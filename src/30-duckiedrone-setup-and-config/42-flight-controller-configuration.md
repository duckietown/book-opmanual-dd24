```{seo}
:description: Configure the Flight Controller of a Duckiedrone DD24-B with QGroundControl and the Duckietown parameter file.
:keywords: Duckiedrone, DD24-B, flight controller, FC, QGroundControl, PX4, Mamba F405 MK2 V2, preset parameters
```

```{needget}
- A base station computer

- An initialized Flight Controller running PX4

- ESCs already flashed with Bluejay

- A data-capable USB-A-to-USB-C cable and any required adapter
---
- A Flight Controller with the Duckiedrone parameters loaded
```

(dd24-b-fc-config)=
# Configuring the Flight Controller

In the previous step, the Flight Controller was prepared for configuration by flashing the bootloader and installing PX4. It is now time to access the Flight Controller and configure it for the Duckiedrone DD24-B.

This page describes how to install QGroundControl, connect to the Flight Controller over USB, and configure the vehicle parameters from a Duckietown preset parameters file.

```{danger}
Before beginning, **remove the propellers** and **disconnect the battery from the Duckiedrone**.
```

(qgroundcontrol-installation)=
## Installing QGroundControl

- Go to the [QGroundControl website](https://qgroundcontrol.com/) and download the installer for the base station operating system (Windows, macOS, or Linux).

- **Windows**: Run the installer and follow the prompts.

- **macOS**: Download the `.dmg` file, open it, and drag the QGroundControl icon into the Applications folder.

- **Linux**: Follow the package manager or AppImage instructions provided on the QGroundControl download page.

- Once installed, launch QGroundControl.

(qgroundcontrol-connection)=
(dd24-b-fc-config-connect)=
## Connecting to the Flight Controller

Connect to the Duckiedrone over USB:

- Connect a data-capable USB-A-to-USB-C cable from the base station to the Flight Controller.

- Open QGroundControl on the base station.

- QGroundControl automatically detects and connects to the Flight Controller over USB.

- Wait a few moments for the top toolbar to show that the vehicle is connected. QGroundControl will then display the summary page.

   ```{figure} ../_images/fc-setup/qgc-vehicle-setup.png
   :align: center
   :width: 100%
   :alt: QGroundControl vehicle setup screen showing red Airframe and Sensors sections
   ```

- The **Airframe** and **Sensors** sections are red, indicating that they still need to be configured.

## Load the preconfigured parameter set

1. Open the **Parameters** tab and load the `.params` file:

   ```{important}
   Import **exactly** this file: {download}`duckiedrone-px4-v4.params <../_static/dd24-b-fc-parameters/duckiedrone-px4-v4.params>`
   ```

   - Click the **Parameters** tab from the left panel to view the configurable parameters for the vehicle.

   - In the Parameters screen, click on the **Tools** menu in the top-right corner.

   - Select **Load from file for review…** from the dropdown menu.

   - Browse to the location of the `.params` file on the base station, select it, and click **Open**.

   - QGroundControl will load and apply the parameters from the file to the vehicle. No errors are reported during this step.

   ```{figure} ../_images/fc-setup/dd24-b/fc-params-load-dd24-b.jpg
   :align: center
   :width: 100%
   :alt: Uploading parameters to the Duckiedrone Flight Controller
   :name: fc-params-load-dd24-b

   Load the downloaded parameter file by following these steps.
   ```

## Reboot the vehicle

- After loading the parameters, reboot the Flight Controller for changes to take effect.

- Select **Reboot Vehicle** from the **Tools** menu.

- After rebooting, reconnect to the vehicle. QGroundControl then shows a summary page similar to the one below:

   ```{figure} ../_images/fc-setup/qgc-summary-post-params.png
   :align: center
   :width: 100%
   :alt: State of the Duckiedrone after reboot
   :name: qgc-summary-post-params

   QGroundControl summary page after uploading the Flight Controller parameters, before performing calibrations.
   ```

````{admonition} Enabling vision fusion later
:class: dropdown

The shipped param file sets `EKF2_EV_CTRL = 0` so the EKF does not try to fuse vision before a VIO is online. Once a VIO publishes `VISION_POSITION_ESTIMATE` / `ODOMETRY` over MAVLink, raise `EKF2_EV_CTRL` to `7` (fuse vision position and velocity) or `15` (also fuse vision yaw, recommended on this magless airframe).
````

````{admonition} Why the Radio page stays red
:class: dropdown

This is expected. The Duckiedrone has no RC transmitter; the Flight Controller is commanded over MAVLink from the Raspberry Pi, so no radio configuration is needed.
````

```{todo}
Re-record the parameter-loading walkthrough video for PX4 (the previous Vimeo capture targeted ArduPilot/QGroundControl).
```

(dd24-b-fc-config-tips)=
## Next step

On-board calibration is mandatory: the shipped `.params` file deliberately omits all `CAL_*` (accelerometer and gyroscope calibration) entries because those are tied to the sensor IDs of a specific board. After loading the parameters, continue to [](dd24-sensor-calibration) to calibrate the sensors on the actual Flight Controller.

(dd24-fc-tuning)=
## Flight Controller tuning

The Flight Controller stabilizes the Duckiedrone with proportional-integral-derivative (PID) controllers, which use feedback from sensors such as the IMU to follow the commanded attitude and yaw rate. The supplied `duckiedrone-px4-v4.params` file already contains the tuning for the Duckiedrone DD24-B, so no manual tuning is needed.

After loading the parameter file and [calibrating the sensors](dd24-sensor-calibration), the Duckiedrone should respond smoothly to roll, pitch, and yaw commands in a test flight, without persistent oscillation or unintended rotation. See [](dd24-flying) for the test flight, and [](dd24-troubleshooting-flight) if the Duckiedrone oscillates or drifts.

```{warning}
The Betaflight PID tuning workflow and its recommended values do not apply to the Duckiedrone DD24-B. Do not use Betaflight Configurator or copy Betaflight-specific PID values onto the PX4 Flight Controller.
```

A validated per-axis PX4 PID-tuning procedure for the Duckiedrone DD24-B is not available yet.

(dd24-b-fc-config-faq)=
## Troubleshooting

```{trouble}
QGroundControl reports that a parameter failed to load.
---
The `duckiedrone-px4-v4.params` file loads cleanly in a single pass. A parameter that fails to load means that the Flight Controller runs the wrong firmware build, or that an outdated parameter file was used. Flash the firmware build named in [](fc-init-flash-px4), then load `duckiedrone-px4-v4.params` again.
```

```{trouble}
The instructions on this page do not work as described.
---
The Duckietown team is happy to help and to hear feedback. Ask a question in the [duckietown-sky-help](https://duckietown.slack.com/archives/CJWNCG667) Slack channel. See [instructions for joining the Duckietown Slack workspace](https://docs.duckietown.com/ente/duckietown-manual/10-setup/01-accounts/duckietown-slack-account.html).
```
