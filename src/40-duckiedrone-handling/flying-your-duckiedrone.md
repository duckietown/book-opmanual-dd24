```{seo}
:description: Fly your Duckiedrone DD24-B from the Duckietown Dashboard using the arming widget, the virtual joystick, and the STABILIZED / LOITER / ALTITUDE / OFFBOARD flight-mode selector.
:keywords: Duckiedrone, DD24, fly duckiedrone, Duckietown Dashboard, mavros arming, PX4 flight modes, STABILIZED, manual flight, OFFBOARD, ALTCTL, LOITER
```

(flying_your_drone)=
# Flying your Duckiedrone

```{needget}
* Fully assembled Duckiedrone with [flight controller initialized](dd24-fc-init)
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
*   The surface you are flying over is not reflective and is not uniform — the visual-odometry node needs texture to lock on. Ideally, place a highly textured poster or a patterned carpet underneath the Duckiedrone.

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

## Power up and open the Dashboard

1.  Plug the battery into your Duckiedrone. The Raspberry Pi will boot up.
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
*   A four-button **FLIGHT MODE** selector: `STABILIZED`, `LOITER`, `ALTITUDE`, `OFFBOARD`.
*   A red **KILL** switch that cuts motor power immediately when clicked.

The widget reflects the live state — it polls `/mavros/state` and refreshes its ARM and FLIGHT MODE indicators whenever the flight controller state changes. If the toggle flips on its own, that reflects a real transition on the flight controller (for example, an auto-disarm).

### Flight modes

The Duckiedrone runs PX4 through MAVROS. The Dashboard exposes four of PX4's flight modes:

| Mode | PX4 name | When to use |
|---|---|---|
| `STABILIZED` | `STABILIZED` | PX4 keeps the Duckiedrone level when roll and pitch are neutral, but throttle is controlled directly, with no altitude or position hold. Use this for the first manual flight: it needs only the IMU attitude estimate, so it arms reliably on a GPS-less Duckiedrone. |
| `LOITER` | `AUTO.LOITER` | Ground-safe default. The Duckiedrone is idle and armable but will not accept manual control input. |
| `ALTITUDE` | `ALTCTL` | PX4 holds altitude automatically. Roll and pitch come from the keyboard (`W`/`A`/`S`/`D`); yaw comes from the on-screen gimbal or arrow keys (or from a physical RC). Use this mode to let PX4's internal attitude-control loops handle stabilization. |
| `OFFBOARD` | `OFFBOARD` | PX4 tracks setpoints published by an external node on `/mavros/setpoint_*`. Use this mode when an external controller is publishing setpoints; PX4 then only handles low-level attitude control. |

```{important}
PX4 only **accepts** an `OFFBOARD` request when it is already receiving setpoints on `/mavros/setpoint_raw/local` (or an equivalent topic) at **>2 Hz**. If no setpoints are being published, PX4 will silently ignore the mode switch and the Duckiedrone will stay in its previous mode. Start your setpoint publisher **before** clicking `OFFBOARD`.

`STABILIZED` and `ALTITUDE` similarly require valid manual-control input (stick values on `/mavros/manual_control/send`) or they will fall back to `LOITER`.
```

```{warning}
In `STABILIZED` the throttle is **fully manual**. PX4 does not hold height for you, so if you lower or release the throttle the Duckiedrone descends. Keep a hand on the throttle at all times and be ready to hit **KILL**.
```

### The Remote Control (virtual joystick) widget

Next to the arming widget, the **Remote Control** widget publishes stick values to `/mavros/manual_control/send`:

*   **On-screen gimbal** (right side of the widget, mouse- or touch-draggable): yaw (left/right) and throttle (up/down). Throttle rests at the bottom (`0`) and *holds* wherever released; yaw springs back to center.
*   **Roll / Pitch indicator** (left side of the widget): shows roll and pitch as arrows, but is keyboard-only. There is no draggable stick for these two axes. Drive them with `W`/`A`/`S`/`D`, see [Keyboard control](#keyboard-control) below.

In `STABILIZED` and `ALTITUDE` modes these fly the Duckiedrone; in `OFFBOARD` mode they are ignored (your setpoint publisher takes over).

### Keyboard control

The **Remote Control** widget also accepts keyboard input. Keyboard and mouse drive the same joystick state and publish to the same `/mavros/manual_control/send` topic, so either can be used at any time, even mid-flight.

| Keys | Axis | Behavior |
|---|---|---|
| `W` / `S` | Pitch | Moves fully forward or back while held; returns to center on release. |
| `A` / `D` | Roll | Moves fully left or right while held; returns to center on release. |
| `←` / `→` | Yaw | Turns left or right while held; returns to center on release. |
| `↑` / `↓` | Throttle | Rises or falls gradually while held, and stays at that level once released, the same as the on-screen gimbal. |
| `Space` | None | Disarms immediately, from anywhere on the page. |

The legend printed at the bottom of the widget repeats these bindings, and hovering over any bar shows the matching tooltip.

#### Throttle ramp: hover threshold and thrust cap

Because the throttle keys ramp rather than jump straight to a value, the widget shows a vertical throttle gauge next to the bars with two calibration fields:

* **Hover**: the throttle value at which this specific Duckiedrone leaves the ground. Below it, `↑` / `↓` step throttle in coarse increments for a quick climb; at or above it, steps become fine for gentle hover trim. Marked as the **blue line** on the gauge.
* **Thrust Cap**: a hard ceiling on throttle. The published throttle value can never exceed it, whatever the keyboard ramp or the joystick asks for. Marked as the **red line** on the gauge.

Both fields are saved in the browser and persist across page reloads, but not across different browsers or devices. Recalibrate when flying from a new machine.

```{warning}
`Thrust Cap` defaults to half throttle, a conservative safety limit. Raise it only after the hover threshold is confirmed, and only as much as the airframe actually needs.
```

To calibrate on a new Duckiedrone:

1.  Arm in `STABILIZED` (see below) with `Thrust Cap` at its default.
1.  Hold `↑` in short bursts and watch the throttle gauge and bar. Note the value at which the Duckiedrone just leaves the ground.
1.  Enter that value into **Hover**. The gauge's blue line and the coarse/fine ramp switchover now match this airframe.
1.  Raise **Thrust Cap** only if the Duckiedrone needs more than half throttle to fly, and disarm and retest after every change.

## First flight

```{danger}
Be prepared to hit the **KILL** switch at any moment. The kill switch will disarm the Duckiedrone instantly regardless of the current mode.
```

1.  Place the Duckiedrone on your textured surface, camera forward.
1.  Verify on the Mission Control page that:
    *   The `FLIGHT MODE` indicator shows `LOITER`.
    *   The altitude trace is centered around `0 m`.
    *   The IMU orientation indicator is level.
1.  Click the **ARM** toggle. The motors will start spinning at idle RPM.
    *   If the motors spin fast, or you hear strange noises, **immediately** click the **KILL** switch.
    *   If the motors do not spin at all, read [](dd24-troubleshooting-faq).
1.  Click the toggle again to disarm. The motors should stop almost immediately. Repeat arming and disarming once or twice to verify responsiveness.
1.  Arm the Duckiedrone again.
1.  Choose a flight mode:

    ::::{tab-set}

    :::{tab-item} Fly in STABILIZED (recommended first flight)

    1.  In the **Remote Control** widget, make sure the **gimbal** (throttle) is at the **bottom** (`THR: 0`) and centered on yaw.
    1.  In the arming widget, click **STABILIZED**. The `FLIGHT MODE` label should update to `STABILIZED`. If it does not, check that the Remote Control widget is actively publishing.
    1.  Click **ARM**. The motors will start spinning at idle RPM.
    1.  Slowly raise the **throttle** (push the gimbal up, or hold `↑`) until the Duckiedrone lifts off. PX4 keeps it level while you hold the throttle.
    1.  Steer roll and pitch with `W`/`A`/`S`/`D` and rotate with **yaw** (gimbal left/right, or `←`/`→`).
    1.  There is **no altitude hold**. Manage height with the throttle throughout the flight. The keyboard's hover-threshold ramp (see [above](#keyboard-control)) makes fine trim easier once airborne.
    1.  To land, ease the throttle down until the Duckiedrone touches down, then click **DISARM**.
    :::

    :::{tab-item} Fly with PX4's attitude stabilization (auto altitude hold)

    1.  In the **Remote Control** widget, center the sticks and set `Throttle` slightly above the mid-point.
    1.  In the arming widget, click **ALTITUDE**.
        *   The `FLIGHT MODE` label should update to `ALTITUDE`. If it stays on `LOITER`, PX4 rejected the request — check that the Remote Control widget is actively publishing.
    1.  Gradually increase the throttle. The Duckiedrone will ascend and PX4 will hold altitude once the sticks are centered.
    1.  Use `W`/`A`/`S`/`D` for roll/pitch to move horizontally. Yaw (gimbal left/right, or `←`/`→`) rotates the Duckiedrone.
    1.  To land, decrease the throttle gradually. When the Duckiedrone is close to the ground, click **DISARM**.
    :::

    :::{tab-item} Fly OFFBOARD (custom external controller)

    1.  Start your setpoint publisher on the Duckiedrone **before** clicking `OFFBOARD`. A minimal publisher should stream `mavros_msgs/PositionTarget` messages on `/mavros/setpoint_raw/local` at ≥20 Hz.
    1.  Click **OFFBOARD**.
        *   If the flight-mode label flips to `OFFBOARD`, PX4 has accepted external control.
        *   If it snaps back to `LOITER`, the setpoint stream was not running or the setpoint values were outside the accepted envelope.
    1.  Your controller now owns the Duckiedrone. Monitor altitude and motor PWM from the Dashboard.
    1.  To land, command a descent from your controller and click **DISARM** once the Duckiedrone is grounded.
    :::

    ::::

1.  **Always** finish the flight by clicking **DISARM**. If anything goes wrong, click **KILL**.

```{danger}
Do **not** put your hands near the propellers while the Duckiedrone is armed. Always disarm before approaching.
```

## Monitoring during flight

Keep the Mission Control page visible while the Duckiedrone is in the air. Useful widgets:

*   **Altitude** — a sudden dip here while in `ALTITUDE` mode is usually a ToF dropout; the PID controller may oscillate.
*   **Motors PWM** — if all four bars saturate at 2000, the Duckiedrone is calling for maximum thrust, typically a sign that the altitude estimate is wrong.
*   **Heartbeats Monitor** — if any heartbeat goes red during flight, the corresponding node stopped publishing.

## Troubleshooting

```{trouble}
The ARM toggle snaps back to **DISARM** a second after I click it.
---
PX4 rejected the arming request because a preflight check failed. Typical causes on a Duckiedrone DD24-B:

*   The EKF is still converging — wait 10–15 seconds after plugging in the battery.
*   The accelerometer bias is out of range — recalibrate the IMU from QGroundControl.
*   The Duckiedrone is not level — place it on a flat surface and retry.
*   The ToF sensor is returning invalid distances — see [](dd24-troubleshooting-faq).

You can read the rejection reason from the Duckiedrone shell:

    docker logs dt-px4 | tail -20
```

```{trouble}
Clicking **OFFBOARD** does nothing — the mode indicator stays on `LOITER`.
---
PX4 silently rejects `OFFBOARD` when no external setpoint stream is running. Make sure your controller publishes on `/mavros/setpoint_raw/local` (or another valid setpoint topic) at **>2 Hz** *before* you click the button. Once setpoints are flowing, retry the mode switch.
```

```{trouble}
The Duckiedrone auto-disarms a few seconds after arming even though I never clicked `DISARM`.
---
This is PX4's *auto-disarm-on-no-takeoff* safety. If the Duckiedrone is armed on the ground without moving the throttle up, PX4 disarms after ~10 s. Simply re-arm and lift off within the timeout window.
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
*   The ESC/Motor protocol matches what the supplied Duckiedrone parameter file expects.
*   The arming service returned `success=True` — click **DISARM**, reopen the Dashboard, and watch the browser console for error messages.
```

**Congratulations on your first Duckiedrone flight!**
