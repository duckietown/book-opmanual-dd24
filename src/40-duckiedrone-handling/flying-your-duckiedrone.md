```{seo}
:description: Fly your Duckiedrone DD24-B from the Duckietown Dashboard using the arming widget, the virtual joystick, and the STABILIZED / LOITER / ALTITUDE / OFFBOARD flight-mode selector.
:keywords: Duckiedrone, DD24, fly duckiedrone, Duckietown Dashboard, mavros arming, PX4 flight modes, STABILIZED, manual flight, OFFBOARD, ALTCTL, LOITER
```

(flying_your_drone)=
# Flying your Duckiedrone

```{needget}
- Fully assembled Duckiedrone with [flight controller initialized](dd24-fc-init)

- Charged battery

- Base station with the Duckietown Dashboard reachable (see [](environment_setup))

- Safety goggles

- Flat, matte, non-reflective takeoff surface
---
- Your Duckiedrone airborne

- A delighted duckie captain
```

## Environment checks

````{warning}
Flying your Duckiedrone is safe **only** when done in an appropriate environment. Make sure:

- You are in an open space that is free of obstructions.

- You have alerted those around you that you are going to fly and have told them to clear the area.

- You are wearing safety goggles.

- The surface you are flying over is matte and non-reflective. Use a highly textured poster or patterned carpet so the Duckiedrone's motion is easy to observe.

    ```{figure} ../_images/flying/highly_textured_surface.png
    :align: center
    :alt: Highly patterned surface that provides visual texture below the Duckiedrone

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

1. Plug the battery into your Duckiedrone. The Raspberry Pi will boot up.

2. Open `http://ROBOT_NAME.local/` in your browser and click **Mission Control**.

3. Wait for the widgets in the default mission to populate.

4. Confirm that every widget in the default mission is healthy — see [](environment_setup) for the checklist.

```{figure} ../_images/flying/mission_control_overview.png
:align: center
:width: 700px
:alt: Duckietown Dashboard Mission Control page showing heartbeat, motor PWM, remote-control, arm/disarm, altitude, Time-of-Flight, and IMU widgets

The Duckiedrone default mission, before arming.
```

## Understanding the Arming widget

The **Arm / Disarm** widget is the primary flight control. It has three elements:

- An **ARM / DISARM** toggle at the top-left of the widget.

- A four-button **FLIGHT MODE** selector: `STABILIZED`, `LOITER`, `ALTITUDE`, `OFFBOARD`.

- A red **KILL** switch that stops motor outputs immediately when clicked.

The widget reflects the live state — it polls `/mavros/state` and refreshes its ARM and FLIGHT MODE indicators whenever the flight controller state changes. If the toggle flips on its own, that reflects a real transition on the flight controller (for example, an auto-disarm).

### Flight modes

PX4 runs on the Duckiedrone's flight controller, and `ros2-mavros` bridges it to ROS 2. The Dashboard exposes four of PX4's flight modes:

| Mode | PX4 name | When to use |
| --- | --- | --- |
| `STABILIZED` | `STABILIZED` | PX4 keeps the Duckiedrone level when roll and pitch are neutral, but throttle is controlled directly, with no altitude or position hold. Use this for the first manual flight: it needs only the IMU attitude estimate, so it arms reliably on a GPS-less Duckiedrone. |
| `LOITER` | `AUTO.LOITER` | Dashboard's disarmed default selection. PX4 cannot arm this automatic mode without a valid position estimate, which the standard Duckiedrone DD24-B configuration does not provide. Use `STABILIZED` or `ALTITUDE` for manual flight. |
| `ALTITUDE` | `ALTCTL` | PX4 holds altitude when the throttle is centered. Roll and pitch come from the keyboard (`W`/`A`/`S`/`D`); yaw comes from the on-screen gimbal or arrow keys (or from a physical RC). Moving throttle above or below center commands ascent or descent, and horizontal position is not held. |
| `OFFBOARD` | `OFFBOARD` | PX4 tracks setpoints published by an external node on `/mavros/setpoint_*`. Use this mode when an external controller is publishing setpoints; PX4 retains the control loops required for the selected setpoint type. |

```{important}
PX4 only **accepts** an `OFFBOARD` request after it has received a supported external-control stream at `>2 Hz` for more than one second. If no setpoints are being published, PX4 will not enter `OFFBOARD` and will stay in its previous mode. Start your setpoint publisher **before** clicking `OFFBOARD`.

The standard Duckiedrone DD24-B configuration does not provide a horizontal position estimate. Do not command local position setpoints until you have configured a valid position source; select a setpoint type compatible with the estimates available to your controller.

`STABILIZED` and `ALTITUDE` require a manual-control input source, such as the Dashboard's Remote Control widget.
```

```{warning}
In `STABILIZED` the throttle is **fully manual**. PX4 does not hold height for you, so lowering the throttle makes the Duckiedrone descend. Manage throttle throughout the flight and be ready to hit **KILL**.
```

### The Remote Control (virtual joystick) widget

Next to the arming widget, the **Remote Control** widget publishes stick values to `/mavros/manual_control/send`:

- **On-screen gimbal** (right side of the widget, mouse- or touch-draggable): yaw (left/right) and throttle (up/down). Throttle rests at the bottom (`0`) and *holds* wherever released; yaw springs back to center.

- **Roll / Pitch indicator** (left side of the widget): shows roll and pitch as arrows, but is keyboard-only. There is no draggable stick for these two axes. Drive them with `W`/`A`/`S`/`D`, see [Keyboard control](#keyboard-control) below.

In `STABILIZED` and `ALTITUDE` modes these fly the Duckiedrone; in `OFFBOARD` mode they are ignored (your setpoint publisher takes over).

### Keyboard control

The **Remote Control** widget also accepts keyboard input. Keyboard and mouse drive the same joystick state and publish to the same `/mavros/manual_control/send` topic, so either can be used at any time, even mid-flight.

| Keys | Axis | Behavior |
| --- | --- | --- |
| `W` / `S` | Pitch | Moves fully forward or back while held; returns to center on release. |
| `A` / `D` | Roll | Moves fully left or right while held; returns to center on release. |
| `←` / `→` | Yaw | Turns left or right while held; returns to center on release. |
| `↑` / `↓` | Throttle | Rises or falls gradually while held, and stays at that level once released, the same as the on-screen gimbal. |
| `Space` | None | Disarms immediately, from anywhere on the page. |

The legend printed at the bottom of the widget repeats these bindings, and hovering over any bar shows the matching tooltip.

#### Throttle ramp: hover threshold and thrust cap

Because the throttle keys ramp rather than jump straight to a value, the widget shows a vertical throttle gauge next to the bars with two calibration fields:

- **Hover**: the throttle value at which this specific Duckiedrone leaves the ground. Below it, `↑` / `↓` step throttle in coarse increments for a quick climb; at or above it, steps become fine for gentle hover trim. Marked as the **blue line** on the gauge.

- **Thrust Cap**: a hard ceiling on throttle. The published throttle value can never exceed it, whatever the keyboard ramp or the joystick asks for. Marked as the **red line** on the gauge.

Both fields are saved in the browser and persist across page reloads, but not across different browsers or devices. Recalibrate when flying from a new machine.

```{warning}
`Thrust Cap` defaults to half throttle, a conservative safety limit. Raise it only after the hover threshold is confirmed, and only as much as the airframe actually needs.
```

To calibrate on a new Duckiedrone:

1. Arm in `STABILIZED` (see below) with `Thrust Cap` at its default.

2. Hold `↑` in short bursts and watch the throttle gauge and bar. Note the value at which the Duckiedrone just leaves the ground.

3. Enter that value into **Hover**. The gauge's blue line and the coarse/fine ramp switchover now match this airframe.

4. Raise **Thrust Cap** only if the Duckiedrone needs more than half throttle to fly, and disarm and retest after every change.

## First flight

```{danger}
Be prepared to hit the **KILL** switch at any moment. It stops the motor outputs immediately; a flying Duckiedrone will fall, so use it only in an emergency.
```

1. Place the Duckiedrone on the takeoff surface, camera forward.

2. Verify on the Mission Control page that:
    - The `FLIGHT MODE` indicator shows `LOITER`.

    - The altitude trace is updating and stable while the Duckiedrone is still.

    - The IMU orientation indicator is level.

3. Click **STABILIZED** in the arming widget. Confirm that the `FLIGHT MODE` indicator changes to `STABILIZED`.

4. Click the **ARM** toggle. The motors will start spinning at idle RPM.
    - If the motors spin fast, or you hear strange noises, **immediately** click the **KILL** switch.

    - If the motors do not spin at all, see [](dd24-troubleshooting-faq).

5. Click the toggle again to disarm. The motors should stop almost immediately. Repeat arming and disarming once or twice to verify responsiveness.

6. Arm the Duckiedrone again.

7. Choose a flight mode:

    ::::{tab-set}

    :::{tab-item} Fly in STABILIZED (recommended first flight)

    1. In the **Remote Control** widget, make sure the **gimbal** (throttle) is at the **bottom** (`THR: 0`) and centered on yaw.

    2. Confirm that the `FLIGHT MODE` label shows `STABILIZED`. If it does not, click **STABILIZED** and check that the Remote Control widget is actively publishing.

    3. Slowly raise the **throttle** (push the gimbal up, or hold `↑`) until the Duckiedrone lifts off. PX4 keeps it level while you hold the throttle.

    4. Steer roll and pitch with `W`/`A`/`S`/`D` and rotate with **yaw** (gimbal left/right, or `←`/`→`).

    5. There is **no altitude hold**. Manage height with the throttle throughout the flight. The keyboard's hover-threshold ramp (see [above](#keyboard-control)) makes fine trim easier once airborne.

    6. To land, ease the throttle down until the Duckiedrone touches down, then click **DISARM**.
    :::

    :::{tab-item} Fly with PX4's attitude stabilization (auto altitude hold)

    1. In the **Remote Control** widget, center the controls.

    2. In the arming widget, click **ALTITUDE**.
        - The `FLIGHT MODE` label should update to `ALTITUDE`. If it does not, check that the Remote Control widget is actively publishing.

    3. Raise the throttle above the takeoff threshold to lift off. Once airborne, return it near the center to hold altitude.

    4. Use `W`/`A`/`S`/`D` for roll/pitch to move horizontally. Yaw (gimbal left/right, or `←`/`→`) rotates the Duckiedrone.

    5. To land, decrease the throttle gradually. Once the Duckiedrone has touched down, click **DISARM**.
    :::

    :::{tab-item} Fly OFFBOARD (custom external controller)

    1. Start your setpoint publisher on the Duckiedrone **before** clicking `OFFBOARD`. Publish a supported stream at more than `2 Hz` for more than one second. Use a higher rate, such as `20 Hz`, for a smooth control stream, and use only setpoint types compatible with the available estimates.

    2. Click **OFFBOARD**.
        - If the `FLIGHT MODE` label flips to `OFFBOARD`, PX4 has accepted external control.

        - If it does not switch, the setpoint stream may not be running or the selected setpoint type may lack the estimate it requires.

    3. Your controller now supplies the Duckiedrone's setpoints. Monitor altitude and motor PWM from the Dashboard.

    4. To land, command a descent from your controller and click **DISARM** once the Duckiedrone is grounded.
    :::

    ::::

8. **Always** finish the flight by clicking **DISARM**. If anything goes wrong, click **KILL**.

```{danger}
Do **not** put your hands near the propellers while the Duckiedrone is armed. Always disarm before approaching.
```

## Monitoring during flight

Keep the Mission Control page visible while the Duckiedrone is in the air. Useful widgets:

- **Altitude** — if the trace becomes unstable or stops updating while in `ALTITUDE` mode, land and inspect the ToF sensor before flying again.

- **Motors PWM** — if all four bars remain near their maximum while the Duckiedrone is not responding as expected, land and inspect the flight-controller and altitude readings before trying again.

- **Heartbeats Monitor** — if any heartbeat goes red during flight, the corresponding node stopped publishing.

## Troubleshooting

```{trouble}
The ARM toggle snaps back to **DISARM** a second after I click it.
---
PX4 rejected the arming request because a preflight check failed. Typical causes on a Duckiedrone DD24-B:

- The flight stack has not completed startup — wait until the Mission Control widgets are populated and healthy, then retry.

- The accelerometer bias is out of range — recalibrate the IMU from QGroundControl.

- The Duckiedrone is not level — place it on a flat surface and retry.

- The ToF sensor is returning invalid distances — see [](dd24-troubleshooting-faq).

If it still fails, inspect the `ros2-mavros` container logs in Portainer for connection errors.
```

```{trouble}
Clicking **OFFBOARD** does nothing — the mode indicator does not change.
---
PX4 will not enter `OFFBOARD` when no external setpoint stream is running. Make sure your controller publishes on `/mavros/setpoint_raw/local` (or another valid setpoint topic) at `>2 Hz` for at least a second *before* you click the button. Once setpoints are flowing, retry the mode switch.
```

```{trouble}
The Duckiedrone auto-disarms after arming even though I never clicked `DISARM`.
---
PX4 can automatically disarm a vehicle that remains on the ground after arming. The timeout is configured on the flight controller, so do not rely on a fixed duration. Before re-arming, make sure the Remote Control widget is publishing and the flight area is clear.
```

```{trouble}
The Mission Control page shows the widgets but no data is updating.
---
The Dashboard communicates with ROS 2 through `ros2-rosbridge-websocket`. Make sure that container is healthy in Portainer, then restart the Duckiedrone containers.
```

```{trouble}
The motors do not spin at all after arming.
---
Check the flight controller initialization ([](dd24-fc-init)). Verify that:

- The Duckiedrone battery is connected; USB alone does not power the ESCs.

- The USB cable between the Raspberry Pi and the flight controller is seated.

- The ESC/Motor protocol matches what the supplied Duckiedrone parameter file expects.

- With the propellers removed, each motor can be tested from the **Actuators** page in QGroundControl.
```

**Congratulations on your first Duckiedrone flight.**
