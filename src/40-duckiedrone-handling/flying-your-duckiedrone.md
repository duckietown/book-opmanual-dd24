```{seo}
:description: Fly the Duckiedrone DD24-B from the Dashboard in STABILIZED mode with the keyboard, with pointers to OFFBOARD flight in the PID learning experience.
:keywords: Duckiedrone, DD24, fly duckiedrone, Dashboard, arming, PX4 flight modes, STABILIZED, manual flight, OFFBOARD, keyboard control
```

(dd24-flying)=
# Flying the Duckiedrone

```{needget}
- A fully assembled Duckiedrone with a [configured Flight Controller](dd24-b-fc-config)

- A charged battery

- A Duckiedrone with a working software stack: [](dd24-environment-setup)

- Knowledge of the Dashboard controls: [](dd24-dashboard-overview)

- Safety goggles

- A flat, matte, non-reflective takeoff surface
---
- A Duckiedrone airborne

- A delighted duckie captain
```

## Environment checks

````{warning}
Fly the Duckiedrone **only** in an appropriate environment, following the [flight safety rules](prelim-duckiedrone-safety). Make sure that:

- The space is open and free of obstructions.

- Everyone nearby knows a flight is about to start and has cleared the area.

- The pilot, and anyone else present, wears safety goggles.

- The surface below the Duckiedrone is matte and non-reflective. A highly textured poster or patterned carpet makes the motion of the Duckiedrone easy to observe.

    ```{figure} ../_images/flying/highly_textured_surface.png
    :name: fig-flying-textured-surface
    :align: center
    :width: 60%
    :alt: Highly patterned surface that provides visual texture below the Duckiedrone

    Example of a highly textured planar surface.
    ```
````

## Hardware checks

With the battery unplugged:

1. Spin each propeller by hand and check that no wires are in the way. Fasten any wire that gets close to the frame with a zip tie, away from the propellers.

2. Check that the Flight Controller USB cable is plugged into the Raspberry Pi. Any USB port works.

3. Check that the camera flat cable is fully seated on both the Raspberry Pi and the camera.

4. Check the screws on the frame, motors, and propellers, and tighten any that are loose. Vibrations during flight can loosen them over time.

## Power up and check the Dashboard

1. Plug the battery into the Duckiedrone and wait for the Raspberry Pi to boot.

2. Open the Dashboard on the **Mission Control** tab and check that:
    - The top bar reads **Bridge: Connected**.

    - In **Heartbeats Monitor**, the `JOYSTICK` heart is green.

    - **Arm / Disarm** reads `DISARMED`.

    - **Motors PWM** shows all four motors at `0`.

    - **Time-of-Flight**: the `Bottom` line reacts to a hand passing under the Duckiedrone.

    - **IMU - Orientation**: the `Roll`, `Pitch`, and `Yaw` lines move when the Duckiedrone is tilted and turned, and `Roll` and `Pitch` sit near `0` when it is level.

    - **Camera** shows a live image.

    See [](dd24-dashboard-overview) for what each widget shows.

## First flight in STABILIZED

```{danger}
Be ready to click **KILL** at any moment. It stops the motor outputs immediately, so a flying Duckiedrone falls. Use it only in an emergency; to stop the motors on the ground, disarm instead.
```

In `STABILIZED` mode, PX4 keeps the Duckiedrone level but does not hold its height. The throttle is fully manual for the whole flight. See [](dd24-dashboard-overview) for the keys used below.

1. Place the Duckiedrone on the takeoff surface, camera forward.

2. In the **Arm / Disarm** widget, click **STABILIZED**. No flight mode is selected by default. The button becomes highlighted only once PX4 has entered the mode; if it stays unhighlighted, PX4 refused the mode, so do not arm.

3. Click the **ARM / DISARM** toggle. The motors start spinning at idle speed as soon as the Duckiedrone is armed, so keep hands and objects clear of the propellers before clicking.
    - If the motors spin fast or make strange noises, **immediately** disarm with <kbd>Space</kbd>, or click **KILL** if the Duckiedrone does not respond.

    - If the motors do not spin at all, see the troubleshooting at the end of this page.

4. Disarm with the toggle or <kbd>Space</kbd>. The motors stop almost immediately. Arm and disarm once or twice more to check that the Duckiedrone responds.

5. Arm the Duckiedrone again.

6. Take off:
    - **First flight of this Duckiedrone, or from a new browser**: **Thrust Cap** starts deliberately low, so the Duckiedrone cannot take off yet. Raise **Thrust Cap** a little at a time, and after each change hold <kbd>↑</kbd> in short bursts until the Duckiedrone just leaves the ground. Enter that throttle value into **Hover**, then set **Thrust Cap** slightly above it. See [](dd24-dashboard-throttle-ramp) for what both fields do.

    - **Hover and Thrust Cap already set**: hold <kbd>↑</kbd> in short bursts to raise the throttle slowly until the Duckiedrone lifts off.

7. Steer with <kbd>W</kbd>/<kbd>A</kbd>/<kbd>S</kbd>/<kbd>D</kbd> and turn with <kbd>←</kbd>/<kbd>→</kbd>. Keep adjusting the height with <kbd>↑</kbd>/<kbd>↓</kbd>.

8. To land, bring the Duckiedrone down gently: hold <kbd>↓</kbd> in short bursts so it descends slowly, and disarm with the toggle or <kbd>Space</kbd> once it touches down.

```{danger}
Keep hands away from the propellers while the Duckiedrone is armed. Always disarm before approaching it.
```

## Flying in OFFBOARD

In `OFFBOARD` mode, an external controller flies the Duckiedrone by publishing setpoints, and the **Remote Control** widget is ignored. The Duckiedrone switches to `OFFBOARD` automatically once the setpoint stream is running.

`OFFBOARD` is used by the [PID altitude control learning experience](lx-dd-pid-altitude-control). Follow that learning experience to set up the controller and fly in `OFFBOARD`.

## Monitoring during flight

Keep the Mission Control page visible while the Duckiedrone is in the air. If any of these happens, land and investigate before flying again:

- **Time-of-Flight**: the `Bottom` line stops updating or jumps around.

- **Motors PWM**: all four lines stay near their maximum while the Duckiedrone does not respond as expected.

- **Heartbeats Monitor**: the `JOYSTICK` heart stops being green, which means the **Remote Control** widget is no longer publishing.

## Troubleshooting

```{trouble}
The ARM toggle snaps back to DISARMED a second after clicking it.
---
PX4 rejected the arming request because a preflight check failed. Typical causes on a Duckiedrone DD24-B:

- The flight stack has not finished starting. Wait until the Mission Control widgets show data, then retry.

- The accelerometer bias is out of range. Recalibrate the sensors as described in [](dd24-sensor-calibration).

- The Duckiedrone is not level. Place it on a flat surface and retry.

- The Time-of-Flight sensor returns invalid distances. See [](dd24-troubleshooting-faq).

If it still fails, inspect the `ros2-mavros` container logs in Portainer, at `http://ROBOT_NAME.local:9000`, for connection errors.
```

```{trouble}
The Duckiedrone disarms on its own after arming.
---
PX4 automatically disarms a Duckiedrone that stays on the ground for too long after arming. The timeout is set on the flight controller. Arm again when ready to take off, and check that the flight area is clear.
```

```{trouble}
The Mission Control widgets show no data.
---
The Dashboard communicates with ROS 2 through `ros2-rosbridge-websocket`. Make sure that container is healthy in Portainer, at `http://ROBOT_NAME.local:9000`, then restart the Duckiedrone containers.
```

```{trouble}
The motors do not spin at all after arming.
---
Check that:

- The battery is connected. USB alone does not power the ESCs.

- The USB cable between the Raspberry Pi and the Flight Controller is seated.

- The ESC protocol matches the supplied Duckiedrone parameter file, as described in [](dd24-b-fc-config).

- With the propellers removed, each motor spins from the **Actuators** page in QGroundControl, as described in [](dd24-motor-configuration).
```

**Congratulations on the first Duckiedrone flight.**
