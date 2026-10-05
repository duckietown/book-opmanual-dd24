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

## Setup checks

These checks are done once, before the first flight of a Duckiedrone. Every setup step below must be complete. Before proceeding, make sure that:

- The ESCs are initialized: [](dd24-esc-init).

- The Flight Controller runs the PX4 firmware: [](dd24-fc-init).

- The Duckiedrone parameter file is loaded on the Flight Controller: [](dd24-b-fc-config).

- The gyroscope, accelerometer, and level horizon are calibrated: [](dd24-sensor-calibration).

- The motor order and spin directions are verified with the propellers removed: [](dd24-motor-configuration).

- The software stack is up to date and passes its checkpoints: [](dd24-environment-setup).

Repeat all the checks from here to the end of [](dd24-flying-arm-disarm-test) before every flight session.

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

5. Check that the arrows embossed on the propellers are visible from above.

6. Check that each propeller matches the spin direction of its motor, listed in [](dd24-motor-configuration). The two propeller types are described in [](component-propellers-cw-diatone-polycarbonate-4040). A propeller on the wrong motor flips the Duckiedrone at takeoff.

7. Check that no propeller is chipped, cracked, or bent, and replace any that is.

8. Check that the battery is charged and secured to the frame. PX4 refuses to arm when the battery charge is below 20%.

## Power up and check the Dashboard

Plug the battery into the Duckiedrone and wait until the Dashboard loads at `http://ROBOT_NAME.local/`.

```{testexpect}
Open the Dashboard on the **Mission Control** tab. Pass a hand under the Duckiedrone, then tilt and turn it.
---
- The top bar reads **Bridge: Connected**.

- In **Heartbeats Monitor**, the `JOYSTICK` heart is green.

- **Arm / Disarm** reads `DISARMED`.

- **Motors PWM** shows all four motors at `0`.

- **Time-of-Flight**: the `Bottom` line reacts to the hand.

- **IMU - Orientation**: the `Roll`, `Pitch`, and `Yaw` lines move with the Duckiedrone, and `Roll` and `Pitch` sit near `0` when it is level.

- **Camera** shows a live image.
```

See [](dd24-dashboard-overview) for what each widget shows.

(dd24-flying-arm-disarm-test)=
## Arm and disarm test

```{danger}
There are two ways to stop the motors, and they do not behave the same:

- <kbd>Space</kbd> resets the throttle to `0` and asks PX4 to disarm. The **ARM / DISARM** toggle only asks PX4 to disarm. PX4 can refuse to disarm while the Duckiedrone is in the air. The Dashboard then shows a **Disarming failed** message and the motors keep running.

- **KILL** forces the disarm. The motors stop immediately, so a flying Duckiedrone falls. Use it only in an emergency.

On the ground, disarm with <kbd>Space</kbd>. In the air, be ready to click **KILL** at any moment.
```

In `STABILIZED` mode, PX4 keeps the Duckiedrone level but does not hold its height. The throttle is fully manual for the whole flight. See [](dd24-dashboard-overview) for the keys used below.

1. Place the Duckiedrone on the takeoff surface, camera forward.

2. In the **Remote Control** widget, check that the throttle gauge reads `0`. If it does not, press <kbd>Space</kbd>.

3. In the **Arm / Disarm** widget, click **STABILIZED**. No flight mode is selected by default. The button becomes highlighted only once PX4 has entered the mode; if it stays unhighlighted, PX4 refused the mode, so do not arm.

4. Click the **ARM / DISARM** toggle. The motors start spinning at idle speed as soon as the Duckiedrone is armed, so keep hands and objects clear of the propellers before clicking.
    - If the motors spin fast or make strange noises, **immediately** disarm with <kbd>Space</kbd>, or click **KILL** if the Duckiedrone does not respond.

    - If the motors do not spin at all, see the troubleshooting at the end of this page.

5. Disarm with the toggle or <kbd>Space</kbd>. The motors stop almost immediately. Arm and disarm once or twice more to check that the Duckiedrone responds.

6. Arm once more and click **KILL**. The motors stop and the toggle returns to `DISARMED`. After **KILL**, the Duckiedrone is disarmed and can be armed again as usual.

### Checkpoint 1 ✅

Do not take off until this checkpoint passes:

```{testexpect}
Complete steps 3 to 6 above.
---
- The **STABILIZED** button becomes highlighted.

- After arming, the toggle reads `ARMED`, all four motors spin at idle speed, and the **Motors PWM** lines leave `0`.

- After <kbd>Space</kbd>, the toggle reads `DISARMED`, the motors stop, and the **Motors PWM** lines return to `0`.

- After **KILL**, the toggle reads `DISARMED` and the motors stop.
```

## Monitoring during flight

Keep the Mission Control page visible while the Duckiedrone is in the air. If any of these happens, land and investigate before flying again:

- **Time-of-Flight**: the `Bottom` line stops updating or jumps around.

- **Motors PWM**: all four lines stay near their maximum while the Duckiedrone does not respond as expected.

- The Duckiedrone descends on its own. The battery is critically low and PX4 is landing the Duckiedrone. Let it land, disarm, and charge the battery.

- **Heartbeats Monitor**: the `JOYSTICK` heart stops being green, which means the **Remote Control** widget is no longer publishing.

## First flight in STABILIZED

1. Arm the Duckiedrone again.

2. Take off:
    - **First flight of this Duckiedrone, or from a new browser**: **Thrust Cap** starts deliberately low, so the Duckiedrone cannot take off yet. Raise **Thrust Cap** a little at a time, and after each change hold <kbd>↑</kbd> in short bursts until the Duckiedrone just leaves the ground. PX4 disarms a Duckiedrone that has not taken off about 10 seconds after arming, so expect to arm again between attempts. Read the percentage printed under the throttle gauge at liftoff, enter it into **Hover**, then set **Thrust Cap** slightly above it. See [](dd24-dashboard-throttle-ramp) for what both fields do.

    - **Hover and Thrust Cap already set**: hold <kbd>↑</kbd> in short bursts to raise the throttle slowly until the Duckiedrone lifts off.

3. Steer with <kbd>W</kbd>/<kbd>A</kbd>/<kbd>S</kbd>/<kbd>D</kbd> and turn with <kbd>←</kbd>/<kbd>→</kbd>. Keep adjusting the height with <kbd>↑</kbd>/<kbd>↓</kbd>.

4. To land, bring the Duckiedrone down gently: hold <kbd>↓</kbd> in short bursts so it descends slowly, and disarm with the toggle or <kbd>Space</kbd> once it touches down. If the Dashboard shows **Disarming failed**, the Duckiedrone is not yet on the ground: keep lowering the throttle and disarm again.

```{danger}
Keep hands away from the propellers while the Duckiedrone is armed. Always disarm before approaching it.
```

### Checkpoint 2 ✅

After landing, before approaching the Duckiedrone:

```{testexpect}
Read the **Arm / Disarm** and **Motors PWM** widgets.
---
- **Arm / Disarm** reads `DISARMED`.

- **Motors PWM** shows all four motors at `0`.
```

Unplug the battery before handling the Duckiedrone.

**Congratulations on the first Duckiedrone flight.**

## Flying in OFFBOARD

In `OFFBOARD` mode, an external controller flies the Duckiedrone by publishing setpoints, and the **Remote Control** widget is ignored. The Duckiedrone switches to `OFFBOARD` automatically once the setpoint stream is running.

`OFFBOARD` is used by the [PID altitude control learning experience](lx-dd-pid-altitude-control). Follow that learning experience to set up the controller and fly in `OFFBOARD`. Complete a flight in `STABILIZED` and pass both checkpoints above first.

## Troubleshooting

```{trouble}
The ARM toggle snaps back to DISARMED a second after clicking it.
---
PX4 rejected the arming request. The Dashboard shows an **Arming failed** message with the reason PX4 reported, such as `Pre-flight checks failed` or `Command denied`. Go through the causes below in order, and retry arming after each one:

1. The throttle is not at zero. PX4 refuses to arm unless the throttle gauge in the **Remote Control** widget reads `0`. Press <kbd>Space</kbd> to reset it.

2. PX4 is not receiving the **Remote Control** commands. Keep the **Mission Control** tab open and in the foreground, and check that the `JOYSTICK` heart in **Heartbeats Monitor** is green.

3. No flight mode is active. Click **STABILIZED** and wait for the button to become highlighted before arming.

4. The flight stack has not finished starting. Wait until the **Motors PWM**, **Time-of-Flight**, and **IMU - Orientation** widgets show data.

5. The Duckiedrone is not level, or it moved while arming. Place it on a flat surface and keep it still.

6. The sensor calibration is missing or out of range. Recalibrate the sensors as described in [](dd24-sensor-calibration).

7. The Time-of-Flight sensor returns invalid distances. Check that the `Bottom` line in the **Time-of-Flight** widget shows a steady value, and see [](dd24-troubleshooting-sensors) if it does not.

8. The battery charge is below 20%. Charge the battery.

If arming still fails, connect QGroundControl as described in [](qgroundcontrol-connection). It reports which preflight check is failing.
```

```{trouble}
The Duckiedrone disarms on its own after arming.
---
PX4 automatically disarms a Duckiedrone that sits idle on the ground and has not taken off about 10 seconds after arming. This is a safety feature, not a fault. Arm again when ready to take off.
```

```{trouble}
The Mission Control widgets show no data.
---
The Dashboard is not receiving ROS 2 data. Reload the Dashboard page, then check the `ros2-rosbridge-websocket` container as described in [](dd24-troubleshooting-containers).
```

```{trouble}
The Duckiedrone flips or tilts hard as soon as it leaves the ground.
---
Disarm immediately. A propeller is on the wrong motor, or a motor is in the wrong position or spins the wrong way. Repeat the propeller checks in the hardware checks above, then see [](dd24-troubleshooting-flight).
```

```{trouble}
The Duckiedrone does not lift off, even with the throttle at the Thrust Cap.
---
**Thrust Cap** starts deliberately low. Raise it a little at a time as described in the first flight steps. If the Duckiedrone still does not lift off, see [](dd24-troubleshooting-flight).
```

```{trouble}
The Duckiedrone climbs too fast.
---
**Thrust Cap** is too far above **Hover**. Land, lower **Thrust Cap** until it is only slightly above **Hover**, and raise the throttle in short bursts. See [](dd24-dashboard-throttle-ramp).
```

```{trouble}
The motors do not spin at all after arming.
---
Check that the battery is connected, since USB alone does not power the ESCs. Then follow the motor checks in [](dd24-troubleshooting-flight).
```
