```{seo}
:description: Set up your Duckiedrone (DD24) software stack so the Duckietown Dashboard is reachable and ready for flight.
:keywords: Duckiedrone, DD24, Duckietown Dashboard, software setup, mavros, PX4, rosbridge, ente
```

(environment_setup)=
# Preparing the software stack

Before you can fly your Duckiedrone, you need an up-to-date software stack running on the drone and a browser on your base station that can reach the Duckietown Dashboard.

```{needget}
*   A fully assembled `DD24` with a [configured Flight Controller](dd24-fc-init)
*   A base station on the same network as the drone (see [](first_connection))
*   The Duckietown Shell (`dts`) installed on the base station
---
*   A drone ready to fly from the Duckietown Dashboard
```

```{attention}
This chapter replaces the legacy `pidrone_pkg` / `screen` workflow. On the `ente` distribution, the flight code runs inside Duckietown containers and is controlled from the Duckietown Dashboard. You no longer need to `ssh` into the drone or start any scripts by hand.
```

## 1. Update the Duckietown Shell

On the base station, make sure you are on the `ente` profile and that `dts` itself is current.

```bash
dts profile list          # 'ente' should be the active profile
pipx upgrade duckietown-shell
dts update
dts desktop update
```

## 2. Update the drone software

Pull the latest Duckiedrone containers onto the drone. This can take several minutes the first time.

```bash
dts duckiebot update -t duckiedrone --distro=ente -f ROBOT_NAME
```

```{note}
Replace `ROBOT_NAME` with the hostname you assigned to your Duckiedrone. On the default image, the hostname is `amelia`.
```

Wait for the command to terminate before continuing. When it finishes, the drone is running:

*   `dashboard` — the web UI you will use to fly
*   `ros2-mavros` — the MAVROS bridge between ROS 2 and the PX4 flight controller
*   `mavlink-proxy`, `driver-tof`, `state-estimator`, `pid-controller`, `visual-odometry` — the flight stack

## 3. Open the Duckietown Dashboard

On the base station, open a browser and navigate to

```
http://ROBOT_NAME.local/
```

The landing page is the Duckietown Dashboard. The first time you open it on a freshly-flashed robot, it will show a four-step **setup wizard**. Complete the steps until you land on the robot info page.

```{tip}
If the page does not load, confirm the drone is reachable with `ping ROBOT_NAME.local`. See [](first_connection) for network troubleshooting.
```

## 4. Verify the flight stack is healthy

On the robot info page, click **Mission Control** in the top navigation bar. The default mission is a grid of widgets showing the live state of the drone:

```{figure} ../_images/flying/mission_control_overview.png
:align: center
:width: 700px

The default Duckiedrone mission, before arming.
```

Before the first flight, confirm:

*   **Heartbeats Monitor** (top left) — the `JOYSTICK`, `ALTITUDE`, `STATE` and `PID` indicators are all green. A red indicator means the matching node is not publishing.
*   **Motors PWM** — four bars are visible, all close to the minimum (~1000). If no bars appear, the Flight Controller node is not streaming motor commands.
*   **Altitude** — the blue trace updates as you gently move the drone up and down.
*   **Time-of-Flight** — the distance reading (in metres) reacts to your hand passing under the drone.
*   **IMU – Orientation** — the attitude indicator tilts when you tilt the drone.
*   **Camera** — the image stream shows what the Duckiedrone sees.

```{tip}
If the **Arm / Disarm** widget shows nothing under `FLIGHT MODE`, it means `/mavros/state` has not published yet. Reload the page after a few seconds; if the issue persists, see [](dd24-troubleshooting-faq).
```

When all widgets are populated, the software stack is healthy and you can proceed to [](flying_your_drone).
