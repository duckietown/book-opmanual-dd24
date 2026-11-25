```{seo}
:description: Overview of the Duckiedrone Dashboard, covering its tabs, signing in, the Overview tab, and each Mission Control widget used to monitor and fly the Duckiedrone DD24-B.
:keywords: Duckiedrone, DD24, Dashboard, Mission Control, Overview, sign in, widgets, arming, flight modes, virtual joystick, keyboard control
```

```{needget}
- A Duckiedrone with the Dashboard reachable: [](dd24-environment-setup)

- A Duckietown account
---
- A signed-in Dashboard, ready to control the Duckiedrone

- Knowledge of the Overview tab and of every Mission Control widget
```

(dd24-dashboard-overview)=
# Dashboard overview

The Dashboard is the web UI used to monitor and fly the Duckiedrone. It opens at `http://ROBOT_NAME.local/`. The bar at the top of every page shows the name of the Duckiedrone, a switch between the light and dark themes, and the **Sign in** button. Below it, the Dashboard is split into tabs:

```{figure} ../_images/dashboard/dashboard-tabs.png
:name: fig-dashboard-dashboard-tabs
:align: center
:width: 90%
:alt: Dashboard tabs Overview, Mission Control, Health, Calibrations, File Manager, Portainer, and Robot Settings

The tabs of the Dashboard.
```

| Tab | What it is for |
| --- | --- |
| **Overview** | A quick health check of the Duckiedrone: see [](dd24-dashboard-info). |
| **Mission Control** | The widgets used to check and fly the Duckiedrone: see [](dd24-dashboard-mission-control). |
| **Health** | History plots of temperature, CPU frequency, and CPU usage. |
| **Calibrations** | The calibration files stored on the robot, such as the camera intrinsics, with cloud backups. |
| **File Manager** | The files stored on the Duckiedrone. |
| **Portainer** | The containers running on the Duckiedrone, as described in [](dd24-troubleshooting-containers). |
| **Robot Settings** | Robot settings such as the hostname, data-sharing permissions, and robot type. |

(dd24-dashboard-sign-in)=
## Sign in

Without signing in, the Dashboard is read-only. The widgets show live data, but the Duckiedrone ignores every command sent from the Dashboard: arming, flight modes, **KILL**, the **Remote Control** widget, and the **GYRO** and **LEVEL** calibrations.

Sign in before flying:

1. Click **Sign in** at the top right of the Dashboard.

2. Click **Sign in with Duckietown**.

3. Paste the personal Duckietown token and click **Login**. The **Get your token** link opens [hub.duckietown.com/profile](https://hub.duckietown.com/profile/), where the token is shown.

    ```{figure} ../_images/dashboard/dashboard-sign-in.png
    :name: fig-dashboard-sign-in
    :align: center
    :width: 70%
    :alt: Sign in with Duckietown window with the Your Token field, the Get your token link, and the Close and Login buttons

    The **Sign in with Duckietown** window.
    ```

4. The Dashboard opens the profile page, and the user name replaces **Sign in** at the top right. Click the name of the Duckiedrone at the top left to go back to the tabs.

(dd24-dashboard-info)=
## Overview

**Overview** is the first tab shown when the Dashboard opens.

```{figure} ../_images/dashboard/info-tab.png
:name: fig-dashboard-info-tab
:align: center
:width: 90%
:alt: Dashboard Overview tab showing the host, type, configuration, board, model, and firmware, CPU, RAM, and disk bars, temperature, power and thermal status, battery charge, voltage, and current, and the network connection

The **Overview** tab.
```

From top to bottom, it shows:

- **The robot**: host name, type, configuration, board, model, and firmware.

- **Compute**: how much of the Raspberry Pi CPU and RAM is in use, how full the microSD card is, and the CPU temperature. **Power & thermal OK** means the Raspberry Pi has had no power or overheating problems since boot. Otherwise it is replaced by `Under-voltage`, `CPU capped`, or `Throttling` while the problem lasts, and by the same label followed by `(earlier)` once it is over.

- **Battery**: the charge left, the voltage, and the current.

- **Connection**: whether the Duckiedrone is online, the network link it uses (`Ethernet` or WiFi, with the WiFi network name), and its IP address.

(dd24-dashboard-mission-control)=
## Mission Control

**Mission Control** shows a set of widgets called a mission. The `default` mission, shown below, is loaded automatically. Each widget shows its name and the ROS topic it reads. The ⋮ menu at the top right of each widget has **Properties** and **Remove** options.

```{figure} ../_images/dashboard/mission-control-default.png
:name: fig-dashboard-mission-control-default
:align: center
:width: 90%
:alt: Mission Control tab with numbered markers on the top bar (1), Camera (2), Arm / Disarm (3), Battery (4), Remote Control (5), Motors PWM (6), Heartbeats Monitor (7), Joystick Heartbeat (8), Altitude (9), Time-of-Flight (10), and IMU - Orientation (11)

The default Duckiedrone mission, signed in and before arming. The numbers match the table below.
```

| # | Widget | What it shows |
| --- | --- | --- |
| 1 | Top bar | The connected vehicle, the loaded mission, and the bridge status. **Bridge connected** means the Dashboard is receiving ROS 2 data from the Duckiedrone. The buttons on the right (**New**, **Open**, **Save**, **Save as**, **Add block**) create, load, and save missions, and add widgets to the current one. |
| 2 | **Camera** | The live image from the onboard camera. |
| 3 | **Arm / Disarm** | Arms and disarms the Duckiedrone, selects the flight mode, and stops the motors in an emergency: see [](dd24-dashboard-arm-disarm). |
| 4 | **Battery** | The battery charge, in percent, and the battery voltage, from the flight controller on `/mavros/battery`. The fill of the battery icon shrinks and changes color as the charge drops, and a `LOW` label appears at `20%` or below. PX4 refuses to arm when the charge is below `20%`. The widget shows `--` when no battery data has arrived for 10 seconds. |
| 5 | **Remote Control** | The virtual joystick used to fly in `STABILIZED` mode, driven from the keyboard: see [](dd24-dashboard-remote-control). |
| 6 | **Motors PWM** | The output sent to each of the four motors, one line per motor. All four read `0` while the Duckiedrone is disarmed. |
| 7 | **Heartbeats Monitor** | Which nodes are running: see [](dd24-dashboard-heartbeats). |
| 8 | **Joystick Heartbeat** | The heartbeat of the virtual joystick in the **Remote Control** widget. It reads `PUBLISHING`. |
| 9 | **Altitude** | The altitude estimate and its reference, in meters, from the altitude node. |
| 10 | **Time-of-Flight** | The distance measured by each Time-of-Flight sensor, in meters, one line per sensor (`Bottom`, `Front`, `Left`, `Right`, `Top`). The default Duckiedrone software runs only the `Bottom` sensor. |
| 11 | **IMU - Orientation** | The `Roll`, `Pitch`, and `Yaw` angles of the Duckiedrone, in degrees, from the flight controller. The **GYRO** and **LEVEL** buttons start the PX4 gyroscope and level-horizon calibrations, as an alternative to [QGroundControl](dd24-sensor-calibration), and the bar next to them shows the calibration status. |

The **Arm / Disarm**, **Remote Control**, and **Heartbeats Monitor** widgets are used to fly, and are described in detail below.

(dd24-dashboard-arm-disarm)=
### Arm / Disarm

```{figure} ../_images/dashboard/mission-control-arm-disarm.png
:name: fig-dashboard-mission-control-arm-disarm
:align: center
:width: 50%
:alt: Arm / Disarm widget with the DISARMED button, the STABILIZED and OFFBOARD flight mode buttons, and the KILL button

The **Arm / Disarm** widget.
```

The **Arm / Disarm** widget is the primary flight control. It has three columns:

- **ARM / DISARM**, on the left: one button that reads `DISARMED` or `ARMED` and switches between the two when clicked.

- **FLIGHT MODE**, in the middle: two buttons, `STABILIZED` and `OFFBOARD`.

- **ACTIONS**, on the right: a red **KILL** button that stops the motor outputs immediately when clicked.

No flight mode is selected by default. A flight mode button becomes highlighted only once PX4 has entered that mode, and stays unhighlighted if PX4 refuses it.

The widget shows the live state of the flight controller. It reads `/mavros/state` and updates the ARM / DISARM and FLIGHT MODE buttons whenever that state changes. If the ARM / DISARM button changes on its own, the flight controller really changed state, for example after an auto-disarm.

#### Flight modes

PX4 runs on the Duckiedrone flight controller, and `ros2-mavros` bridges it to ROS 2. The Dashboard exposes two PX4 flight modes:

| Mode | When to use |
| --- | --- |
| `STABILIZED` | Manual flight with the **Remote Control** widget. PX4 keeps the Duckiedrone level when roll and pitch are neutral, but the throttle is controlled directly, with no altitude or position hold. It needs only the IMU attitude estimate, so it arms reliably on the Duckiedrone. Use this mode for the first flight. |
| `OFFBOARD` | Flight driven by an external controller, as in the [PID altitude control learning experience](lx-dd-pid-altitude-control). PX4 tracks the setpoints that an external node publishes on `/mavros/setpoint_*`, and the **Remote Control** widget is ignored. |

```{important}
There is no need to click **OFFBOARD**. Once an external setpoint stream is running, the Duckiedrone switches to `OFFBOARD` automatically and the **OFFBOARD** button becomes highlighted. PX4 needs the stream at more than `2 Hz` for more than one second; without it, PX4 stays in the previous mode.
```

```{warning}
In `STABILIZED` the throttle is **fully manual**. PX4 does not hold the height, so lowering the throttle makes the Duckiedrone descend. Manage the throttle throughout the flight and be ready to click **KILL**.
```

(dd24-dashboard-remote-control)=
### Remote Control

```{figure} ../_images/dashboard/mission-control-remote-control.png
:name: fig-dashboard-mission-control-remote-control
:align: center
:width: 90%
:alt: Remote Control widget with the Roll, Pitch, Yaw, and Throttle bars, the Hover and Thrust Cap fields, the throttle gauge, the Roll / Pitch and Yaw / Throttle controllers, and the keyboard legend

The **Remote Control** widget.
```

The **Remote Control** widget publishes stick values to `/mavros/manual_control/send`. It has two controllers, and both are keyboard only. Neither can be dragged with the mouse or by touch, which prevents accidental inputs.

- **Roll / Pitch** controller: shows the roll and pitch commands. Drive it with <kbd>W</kbd>/<kbd>A</kbd>/<kbd>S</kbd>/<kbd>D</kbd>.

- **Yaw / Throttle** controller: shows the yaw (left/right) and throttle (up/down) commands. Drive it with the arrow keys (<kbd>←</kbd>, <kbd>→</kbd>, <kbd>↑</kbd>, <kbd>↓</kbd>). Throttle rests at the bottom (`0`) and *holds* wherever released; yaw springs back to center.

In `STABILIZED` mode these controllers fly the Duckiedrone. In `OFFBOARD` mode they are ignored, and the external setpoint publisher is in control.

(dd24-dashboard-keyboard-control)=
#### Keyboard control

The keyboard is the only input to the **Remote Control** widget. Each key moves one axis of the two controllers.

| Keys | Axis | Behavior |
| --- | --- | --- |
| <kbd>W</kbd> / <kbd>S</kbd> | Pitch | Moves fully forward or back while held; returns to center on release. |
| <kbd>A</kbd> / <kbd>D</kbd> | Roll | Moves fully left or right while held; returns to center on release. |
| <kbd>←</kbd> / <kbd>→</kbd> | Yaw | Turns left or right while held; returns to center on release. |
| <kbd>↑</kbd> / <kbd>↓</kbd> | Throttle | Rises or falls gradually while held, and stays at that level once released. |
| <kbd>Space</kbd> | None | Resets the throttle to `0` and asks PX4 to disarm, from anywhere on the page. PX4 can refuse while the Duckiedrone is in the air; see [](dd24-flying-arm-disarm-test). |

The legend at the bottom of the widget repeats these bindings, and hovering over any bar shows the matching tooltip.

(dd24-dashboard-throttle-ramp)=
#### Throttle ramp: hover threshold and thrust cap

Because the throttle keys ramp rather than jump straight to a value, the widget shows a vertical throttle gauge next to the bars, with the current throttle printed under it, and two calibration fields, both in percent of full throttle:

- **Hover %**: the throttle at which this specific Duckiedrone leaves the ground. Below it, <kbd>↑</kbd> and <kbd>↓</kbd> change the throttle in coarse steps of `2.5%` for a quick climb; at or above it, in fine steps of `1%` for gentle hover trim. Marked as the **blue line** on the gauge.

- **Thrust Cap %**: a hard ceiling on the throttle. The published throttle can never exceed it, whatever the keyboard asks for. Marked as the **red line** on the gauge.

**Hover %** starts at `10` and **Thrust Cap %** at `40`. **Thrust Cap %** starts deliberately low, so the Duckiedrone cannot take off until it is set for that Duckiedrone. Both fields are set during the first flight of each Duckiedrone, as described in [](dd24-flying).

Both fields are saved in the browser and persist across page reloads, but not across different browsers or devices. Recalibrate when flying from a new machine.

```{warning}
Keep **Thrust Cap %** only slightly above **Hover %**. A higher cap lets the throttle go far beyond what the Duckiedrone needs to hover, and the Duckiedrone is powerful enough to shoot up out of control.
```

#### Checkpoint ✅

To verify that the keyboard drives the **Remote Control** widget, sign in and keep the Duckiedrone disarmed, with **Arm / Disarm** reading `DISARMED`:

```{testexpect}
1. Hold <kbd>W</kbd>, <kbd>A</kbd>, <kbd>S</kbd>, <kbd>D</kbd>, <kbd>←</kbd>, and <kbd>→</kbd>, one at a time.

2. Hold <kbd>↑</kbd>.

3. Press <kbd>Space</kbd>.
---
1. The matching controller moves while the key is held and returns to center on release.

2. The throttle gauge rises and stops at the red **Thrust Cap %** line.

3. The throttle gauge returns to `0`.
```

```{attention}
The throttle holds its value and is not reset when the Duckiedrone is armed. Bring it back to `0` with <kbd>Space</kbd> or <kbd>↓</kbd> before arming.
```

(dd24-dashboard-heartbeats)=
### Heartbeats Monitor

```{figure} ../_images/dashboard/mission-control-heartbeats.png
:name: fig-dashboard-mission-control-heartbeats
:align: center
:width: 60%
:alt: Heartbeats Monitor widget with the JOYSTICK and ALTITUDE tiles reading OK and the STATE and PID tiles reading STALE, above the Joystick Heartbeat widget reading PUBLISHING

The **Heartbeats Monitor** and **Joystick Heartbeat** widgets.
```

The **Heartbeats Monitor** has one tile per node: `JOYSTICK`, `ALTITUDE`, `STATE`, and `PID`. A tile reads `OK`, with a filled heart, while its node is publishing, and `STALE`, with an empty heart, when it is not. On the default Duckiedrone software:

- `JOYSTICK` reads `OK` while a signed-in Dashboard has **Mission Control** open. It must read `OK` to fly.

- `ALTITUDE` reads `OK` while the altitude node publishes, which needs a valid distance from the `Bottom` Time-of-Flight sensor.

- `STATE` and `PID` always read `STALE`, because the default software does not run these nodes.
