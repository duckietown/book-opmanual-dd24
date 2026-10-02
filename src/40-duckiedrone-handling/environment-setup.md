```{seo}
:description: Update the Duckiedrone DD24-B software stack and check in the Duckietown Dashboard that it is ready for flight.
:keywords: Duckiedrone, DD24, Duckietown Dashboard, software setup, software update, mavros, PX4, rosbridge, ente
```

(dd24-environment-setup)=
# Preparing the software stack

Flying the Duckiedrone requires up-to-date software on both the base station and the Duckiedrone, and a browser on the base station that can reach the Duckietown Dashboard.

```{needget}
- A fully assembled Duckiedrone DD24-B with a [configured Flight Controller](dd24-b-fc-config)

- A base station on the same network as the Duckiedrone (see [](first_connection))

- The Duckietown Shell (`dts`) installed on the base station

- Several minutes; the exact duration depends on the images to download and the network connection
---
- Updated Duckiedrone containers and base-station tools

- A Duckiedrone ready to fly from the Duckietown Dashboard
```

```{attention}
This chapter replaces the legacy `pidrone_pkg` / `screen` workflow. On the `ente` distribution, the flight code runs inside Duckietown containers and is controlled from the Duckietown Dashboard. You do not need to SSH into the Duckiedrone to start scripts manually.
```

## 1. Update the base station

Check that `ente` is the active Duckietown Shell profile:

```bash
dts profile list
```

Then update the Duckietown Shell, its commands, and the Duckietown desktop software, in this order:

```bash
pipx upgrade duckietown-shell
dts update
dts desktop update
```

## 2. Update the Duckiedrone

Pull the latest containers onto the Duckiedrone, replacing `ROBOT_NAME` with the hostname set during the [microSD card initialization](dd24-sw-init):

```bash
dts duckiebot update ROBOT_NAME
```

The first update can take several minutes. Wait for the command to finish before continuing.

When the update finishes, the Duckiedrone containers start automatically. For example:

- `dashboard`: the web UI used to fly the Duckiedrone.

- `ros2-mavros`: passes commands such as arm and disarm to the flight controller, and its state back to ROS 2.

- `driver-tof-bottom` and `ros2-tof-bottom`: the altitude sensor and its ROS 2 bridge.

- `driver-camera` and `ros2-camera`: the onboard camera and its ROS 2 bridge.

See [](duckiedrone-containers) for the complete list.

## 3. Open the Duckietown Dashboard

On the base station, open a browser and go to:

```text
http://ROBOT_NAME.local/
```

If `ROBOT_NAME.local` does not resolve, use the Duckiedrone IP address instead, shown in the `Address` column of `dts fleet discover`:

```text
http://ROBOT_IP/
```

The first time the Dashboard is opened on a freshly flashed Duckiedrone, it shows a four-step **setup wizard**. Complete the steps until the **Robot > Info** page appears.

After the setup, the Dashboard opens directly on the **Robot > Info** page.

```{figure} ../_images/dashboard/info-tab.png
:name: fig-environment-setup-info-tab
:align: center
:width: 700px
:alt: Duckietown Dashboard Robot Info page showing the robot name, type, configuration, firmware, and temperature, disk, CPU, RAM, frequency, and battery gauges

The **Robot > Info** page of the Duckietown Dashboard.
```

## 4. Check the flight stack

Click the **Mission Control** tab. The default mission is a grid of widgets that show the live state of the Duckiedrone:

```{figure} ../_images/dashboard/mission-control-default.png
:name: fig-environment-setup-mission-control
:align: center
:width: 700px
:alt: Duckietown Dashboard Mission Control page showing heartbeat, motor PWM, remote-control, arm/disarm, altitude, Time-of-Flight, IMU, and camera widgets

The default Duckiedrone mission, before arming.
```

Before the first flight, check that:

- The top bar reads **Bridge: Connected**. This means the Dashboard is receiving ROS 2 data from the Duckiedrone.

- In **Heartbeats Monitor**, the `JOYSTICK` heart is green.

- **Motors PWM** shows all four motors at `0` while the Duckiedrone is disarmed.

- **Time-of-Flight**: the `Bottom` line in the graph changes when a hand passes under the Duckiedrone.

- **IMU - Orientation**: the `Roll`, `Pitch`, and `Yaw` lines in the graph change when the Duckiedrone is tilted sideways, tilted forward or back, and rotated in place.

- **Arm / Disarm** reads `DISARMED`.

- **Camera** shows a live image from the Duckiedrone camera.

```{note}
The `ALTITUDE`, `STATE`, and `PID` heartbeats and the **Altitude** widget read from nodes that are not part of the default Duckiedrone software. They stay empty on a healthy default setup.
```

When all these checks pass, the software stack is ready. Continue to [](flying_your_drone).

## Troubleshooting

```{trouble}
The Dashboard does not load.
---
Check that the Duckiedrone is reachable with `ping ROBOT_NAME.local` or `ping ROBOT_IP`. See [](first_connection) for network troubleshooting.
```

```{trouble}
The Dashboard opens through `ROBOT_IP` but the widgets show no data.
---
Older Dashboard images do not support access by IP address. Update the Duckiedrone as described in step 2.
```
