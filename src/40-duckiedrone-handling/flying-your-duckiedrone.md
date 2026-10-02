```{seo}
:description: Fly your Duckiedrone DD24-B from the Duckietown Dashboard using the arming widget, the virtual joystick, and the STABILIZED / LOITER / ALTITUDE / OFFBOARD flight-mode selector.
:keywords: Duckiedrone, DD24, fly duckiedrone, Duckietown Dashboard, mavros arming, PX4 flight modes, STABILIZED, manual flight, OFFBOARD, ALTCTL, LOITER
```

(flying_your_drone)=
# Flying your Duckiedrone

```{needget}
- Fully assembled Duckiedrone with [flight controller initialized](dd24-fc-init)

- Charged battery

- Base station with the Duckietown Dashboard reachable (see [](dd24-environment-setup))

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

4. Confirm that every widget in the default mission is healthy — see [](dd24-environment-setup) for the checklist.

```{figure} ../_images/flying/mission_control_overview.png
:align: center
:width: 700px
:alt: Duckietown Dashboard Mission Control page showing heartbeat, motor PWM, remote-control, arm/disarm, altitude, Time-of-Flight, and IMU widgets

The Duckiedrone default mission, before arming.
```

## Dashboard controls

The Arm / Disarm and Remote Control widgets, the flight modes, and the keyboard controls are described in [](dd24-dashboard-overview).

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

    5. There is **no altitude hold**. Manage height with the throttle throughout the flight. The keyboard's hover-threshold ramp (see [](dd24-dashboard-keyboard-control)) makes fine trim easier once airborne.

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
