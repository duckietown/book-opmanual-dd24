```{seo}
:description: Update the Duckiedrone DD24-B software stack and check in the Dashboard that it is ready for flight.
:keywords: Duckiedrone, DD24, Dashboard, software setup, software update, mavros, PX4, rosbridge, ente
```

(dd24-environment-setup)=
# Preparing the software stack

Flying the Duckiedrone requires up-to-date software on both the base station and the Duckiedrone, and a browser on the base station that can reach the Dashboard.

```{needget}
- A fully assembled Duckiedrone DD24-B with a [configured Flight Controller](dd24-b-fc-config)

- A base station on the same network as the Duckiedrone (see [](first_connection))

- The Duckietown Shell (`dts`) installed on the base station

- Several minutes; the exact duration depends on the images to download and the network connection
---
- Updated Duckiedrone containers and base-station tools

- A Duckiedrone ready to fly from the Dashboard
```

```{attention}
This chapter replaces the legacy `pidrone_pkg` / `screen` workflow. On the `ente` distribution, the flight code runs inside Duckietown containers and is controlled from the Dashboard. You do not need to SSH into the Duckiedrone to start scripts manually.
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

## 3. Open the Dashboard

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
:alt: Dashboard Robot Info page showing the robot name, type, configuration, firmware, and temperature, disk, CPU, RAM, frequency, and battery gauges

The **Robot > Info** page of the Dashboard.
```

## 4. Check the connection

Click the **Mission Control** tab and check that:

- The top bar reads **Bridge: Connected**.

- In **Heartbeats Monitor**, the `JOYSTICK` heart is green.

Together, these show that the Duckiedrone containers are running and the Dashboard is receiving ROS 2 data.

The software stack is ready. Next, explore the Dashboard in more detail in [](dd24-dashboard-overview), which explains each tab and every widget used to fly the Duckiedrone.

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
