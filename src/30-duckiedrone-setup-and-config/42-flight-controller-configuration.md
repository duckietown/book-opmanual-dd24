```{seo}
:description: Learn how to perform the flight controller preliminary configuration on a DD24-B Duckiedrone.
:keywords: Duckiedrone, DD24-B, flight controller, FC, QGroundControl, PX4, Mamba F405 MK2, preset parameters
```

```{needget}
* A base station computer with QGroundControl installed
* An initialized Flight Controller running PX4 with the parameters already loaded
* ESCs already flashed with Bluejay
* A charged drone battery 
* A USB to USB-C cable with data
---
* An initialized Flight Controller running PX4 with the parameters already loaded
```

(dd24-b-fc-config)=
# Flight Controller Configuration

In the previous step we have prepared the flight controller (FC) for configuration by flashing the bootloader and installing PX4. It is now time to access the FC and configure it for the specifics of the Duckiedrone (`DD24-B`). 

By following these steps, you will be able to install QGroundControl, connect to your flight controller over USB, and configure your vehicle's parameters from a Duckietown preset parameters file.
```{attention}
Before you begin, **remove the propellers** and **disconnect the battery from the drone**.
```

(qgroundcontrol-connection)=
## Installing QGroundControl 

   - Go to the [QGroundControl website](http://qgroundcontrol.com/) and download the installer for your operating system (Windows, macOS, or Linux).
   - Follow the installation instructions for your OS:
     - **Windows**: Run the installer and follow the prompts.
     - **macOS**: Download the `.dmg` file, open it, and drag the QGroundControl icon into your Applications folder.
     - **Linux**: Follow the package manager or AppImage instructions provided on the QGroundControl download page.
   - Once installed, launch QGroundControl.

(dd24-b-fc-config-connect)=
## Connecting to the Flight Controller


Connect to Your Vehicle over USB:
   - Connect a USB-C cable from your computer to the flight controller.
   - Open QGroundControl on your computer.
   - QGroundControl **auto-detects the flight controller over USB and connects automatically**.
   - Wait for a few moments; the top toolbar should show the vehicle as connected, then arrive at this summary page.

   ```{figure} ../_images/fc-setup/qgc-vehicle-setup.png
   ```

   - Here you can see many sections such as **Airframe** and **Sensors** are red, indicating that they need to be configured.


## Load the preconfigured parameters set


3. Open Parameters and load the `.params` file:

   ```{important}
   Import **exactly** this file: {download}`_static/duckiedrone-px4-v3.params <../_static/dd24-b-fc-parameters/duckiedrone-px4-v3.params>`
   ```

   - Click the **Parameters** tab from the left panel to view the configurable parameters for your vehicle.
   - In the Parameters screen, click on the **Tools** menu in the top right corner.
   - Select **Load from file for review…** from the dropdown menu.
   - Browse to the location of your `.params` file on your computer, select it, and click **Open**.
   - QGroundControl will load and apply the parameters from the file to your vehicle. There should be no errors during this step.


## Reboot the vehicle

   - After loading the parameters, it is usually necessary to reboot the flight controller for changes to take effect.
   - You can reboot the vehicle by selecting **Reboot Vehicle** from the **Tools** menu.
   - After rebooting, reconnect to the vehicle. You will then see a summary page similar to the one below:

   ```{figure} ../_images/fc-setup/qgc-summary-post-params.png
   ```


```{note}
The shipped param file sets `EKF2_EV_CTRL = 0` so the EKF does not try to fuse vision before a VIO is online. Once a VIO publishes `VISION_POSITION_ESTIMATE` / `ODOMETRY` over MAVLink, raise `EKF2_EV_CTRL` to `7` (fuse vision pos + vel) or `15` (also fuse vision yaw — recommended on this magless airframe).
```




```{note}
The **Radio** page will stay red — this is expected. The Duckiedrone has no RC transmitter; the flight controller is commanded over MAVLink from the Raspberry Pi, so no radio configuration is needed.
```

```{todo}
Re-record the parameter-loading walkthrough video for PX4 (the previous Vimeo capture targeted ArduPilot/QGC).
```

(dd24-b-fc-config-tips)=
### Additional Tips

- **Check for Errors:** The `duckiedrone-px4-v3.params` file loads cleanly in a single pass. If QGroundControl reports any parameter failing to load, you are on the wrong firmware build or using an outdated param file.
- **On-board calibration is mandatory:** the shipped `.params` file deliberately omits all `CAL_*` (accelerometer/gyro calibration) entries because those are tied to a specific board's sensor IDs. Run the **Sensors** calibration in QGroundControl on the actual flight controller after loading the parameters.

(dd24-b-fc-config-faq)=
## Troubleshooting


```{trouble}
I am having issues following the instructions!
---
We're happy to help and hear your feedback! Please post a question on our Stack Overflow. You can find the instructions on how to join it [here](https://duckietown.slack.com/archives/CHHQJ0E0H/p1670874390660429).

You can also contact us via Slack in the following channel: [duckietown-sky-help](https://duckietown.slack.com/archives/CJWNCG667)
```