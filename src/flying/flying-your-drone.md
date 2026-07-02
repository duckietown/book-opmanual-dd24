```{seo}
:description: Fly your Duckiedrone (DD24) from the Duckietown Dashboard using the arming widget, the virtual joystick, and the LOITER / ALTITUDE / OFFBOARD flight-mode selector.
:keywords: Duckiedrone, DD24, fly duckiedrone, Duckietown Dashboard, mavros arming, PX4 flight modes, OFFBOARD, ALTCTL, LOITER
```

(flying_your_drone)=
# Flying your Duckiedrone

```{needget}
* Fully assembled drone with [flight controller initialized](dd24-fc-init)
* Charged battery
* Base station with the Duckietown Dashboard reachable (see [](environment_setup))
* Safety goggles
* Highly textured planar surface
---
* Your Duckiedrone airborne
* A delighted duckie captain
```

## Environment checks

````{warning}
Flying your Duckiedrone is safe **only** when done in an appropriate environment. Make sure:

*   You are in an open space that is free of obstructions.
*   You have alerted those around you that you are going to fly and have told them to clear the area.
*   You are wearing safety goggles.
*   The surface you are flying over is not reflective and is not uniform — the visual-odometry node needs texture to lock on. Ideally, place a highly textured poster or a patterned carpet underneath the drone.

    ```{figure} ../_images/flying/highly_textured_surface.png
    :align: center

    Example of a highly textured planar surface.
    ```
````

## Hardware checks

If the battery is still connected, unplug it before the following checks.

### Wire management

Spin the props with your finger and make sure there are no wires in the way. If wires get close, use a zip tie to fasten them to the frame, away from the props.

### USB connections

Make sure that the Flight Controller USB cable is plugged into the Raspberry Pi (any of the USB ports is fine). Make sure the camera flat cable is fully seated on both the Pi and the camera side.

## Power up and open the dashboard

1.  Plug the battery into your drone. The Raspberry Pi will boot up.
1.  Wait ~45 seconds for all the Duckiedrone containers to come up.
1.  Open `http://ROBOT_NAME.local/` in your browser and click **Mission Control**.
1.  Confirm that every widget in the default mission is healthy — see [](environment_setup) for the checklist.

```{figure} ../_images/flying/mission_control_overview.png
:align: center
:width: 700px

The Duckiedrone default mission, before arming.
```

## Understanding the Arming widget

The **Arm / Disarm** widget is the primary flight control. It has three elements:

*   An **ARM / DISARM** toggle at the top-left of the widget.
*   A three-button **FLIGHT MODE** selector: `LOITER`, `ALTITUDE`, `OFFBOARD`.
*   A red **KILL** switch that cuts motor power immediately when clicked.

The widget reflects the live state — it polls `/mavros/state` and refreshes its ARM and FLIGHT MODE indicators whenever the flight controller state changes. If the toggle flips on its own, that reflects a real transition on the flight controller (for example, an auto-disarm).

### Flight modes

The drone runs PX4 through MAVROS. The dashboard exposes three of PX4's flight modes:

| Mode | PX4 name | When to use |
|---|---|---|
| `LOITER` | `AUTO.LOITER` | Ground-safe default. The drone is idle and armable but will not accept manual stick input. |
| `ALTITUDE` | `ALTCTL` | PX4 holds altitude automatically; you command roll, pitch and yaw from the virtual joystick (or from a physical RC). Use this when you want PX4's internal attitude control loops to do the stabilization. |
| `OFFBOARD` | `OFFBOARD` | PX4 tracks setpoints published by an external node on `/mavros/setpoint_*`. Use this when you are writing your own controller and want PX4 to only handle low-level attitude control. |

```{important}
PX4 only **accepts** an `OFFBOARD` request when it is already receiving setpoints on `/mavros/setpoint_raw/local` (or an equivalent topic) at **>2 Hz**. If no setpoints are being published, PX4 will silently ignore the mode switch and the drone will stay in its previous mode. Start your setpoint publisher **before** clicking `OFFBOARD`.

`ALTITUDE` similarly requires valid manual-control input (stick values on `/mavros/manual_control/send`) or it will fall back to `LOITER`.
```

### The Remote Control (virtual joystick) widget

Next to the arming widget, the **Remote Control** widget publishes stick values to `/mavros/manual_control/send`. In `ALTITUDE` mode, moving the joystick tilts the drone; in `OFFBOARD` mode the joystick is ignored (your setpoint publisher takes over).

## First flight

```{danger}
Be prepared to hit the **KILL** switch at any moment. The kill switch will disarm the drone instantly regardless of the current mode.
```

1.  Place the drone on your textured surface, camera forward.
1.  Verify on the Mission Control page that:
    *   The `FLIGHT MODE` indicator shows `LOITER`.
    *   The altitude trace is centred around `0 m`.
    *   The IMU orientation indicator is level.
1.  Click the **ARM** toggle. The motors will start spinning at idle RPM.
    *   If the motors spin fast, or you hear strange noises, **immediately** click the **KILL** switch.
    *   If the motors do not spin at all, read [](dd24-troubleshooting-faq).
1.  Click the toggle again to disarm. The motors should stop almost immediately. Repeat arming and disarming once or twice to verify responsiveness.
1.  Arm the drone again.
1.  Choose a flight mode:

    ::::{tab-set}

    :::{tab-item} Fly with PX4's attitude stabilization (recommended first flight)

    1.  In the **Remote Control** widget, centre the sticks and set `Throttle` slightly above the mid-point.
    1.  In the arming widget, click **ALTITUDE**.
        *   The `FLIGHT MODE` label should update to `ALTITUDE`. If it stays on `LOITER`, PX4 rejected the request — check that the Remote Control widget is actively publishing.
    1.  Gradually increase throttle. The drone will ascend and PX4 will hold altitude once the sticks are centred.
    1.  Use roll/pitch on the virtual joystick to move horizontally. Yaw on the secondary joystick rotates the drone.
    1.  To land, decrease the throttle gradually. When the drone is close to the ground, click **DISARM**.
    :::

    :::{tab-item} Fly OFFBOARD (custom external controller)

    1.  Start your setpoint publisher on the drone **before** clicking `OFFBOARD`. A minimal publisher should stream `mavros_msgs/PositionTarget` messages on `/mavros/setpoint_raw/local` at ≥20 Hz.
    1.  Click **OFFBOARD**.
        *   If the flight-mode label flips to `OFFBOARD`, PX4 has accepted external control.
        *   If it snaps back to `LOITER`, the setpoint stream was not running or the setpoint values were outside the accepted envelope.
    1.  Your controller now owns the drone. Monitor altitude and motor PWM from the dashboard.
    1.  To land, command a descent from your controller and click **DISARM** once the drone is grounded.
    :::

    ::::

1.  **Always** finish the flight by clicking **DISARM**. If anything goes wrong, click **KILL**.

```{danger}
Do **not** put your hands near the propellers while the drone is armed. Always disarm before approaching.
```

## Monitoring during flight

Keep the Mission Control page visible while the drone is in the air. Useful widgets:

*   **Altitude** — a sudden dip here while in `ALTITUDE` mode is usually a ToF dropout; the PID controller may oscillate.
*   **Motors PWM** — if all four bars saturate at 2000, the drone is calling for maximum thrust, typically a sign the altitude estimate is wrong.
*   **Heartbeats Monitor** — if any heartbeat goes red during flight, the corresponding node stopped publishing.

## Troubleshooting

```{trouble}
The ARM toggle snaps back to **DISARM** a second after I click it.
---
PX4 rejected the arming request because a preflight check failed. Typical causes on a DD24:

*   The EKF is still converging — wait 10–15 seconds after plugging the battery.
*   The accelerometer bias is out of range — recalibrate the IMU from QGroundControl.
*   The drone is not level — place it on a flat surface and retry.
*   The ToF sensor is returning invalid distances — see [](dd24-troubleshooting-faq).

You can read the rejection reason from the drone logs:

    dts devel run -H ROBOT_NAME -L default -- --rm docker logs dt-px4 | tail -20
```

```{trouble}
Clicking **OFFBOARD** does nothing — the mode indicator stays on `LOITER`.
---
PX4 silently rejects `OFFBOARD` when no external setpoint stream is running. Make sure your controller publishes on `/mavros/setpoint_raw/local` (or another valid setpoint topic) at **>2 Hz** *before* you click the button. Once setpoints are flowing, retry the mode switch.
```

```{trouble}
The drone auto-disarms a few seconds after arming even though I never clicked `DISARM`.
---
This is PX4's *auto-disarm-on-no-takeoff* safety. If the drone is armed on the ground without moving the throttle up, PX4 disarms after ~10 s. Simply re-arm and lift off within the timeout window.
```

```{trouble}
The Mission Control page shows the widgets but no data is updating.
---
Rosbridge may not be running, or it is resolving `~` to the wrong namespace. In the browser, open the widget settings (gear icon on the widget header) and confirm the topic paths start with a `/`, not `~/`. The shipped default mission uses absolute paths; custom missions may not.
```

```{trouble}
The motors don't spin at all after arming.
---
Check the flight controller initialization ([](dd24-fc-init)). Verify that:

*   The USB cable between the Raspberry Pi and the flight controller is seated.
*   The ESC/Motor protocol matches what the DD24 parameter file expects.
*   The arming service returned `success=True` — click **DISARM**, reopen the dashboard, and watch the browser console for error messages.
```

**Congratulations on your first Duckiedrone flight!**
