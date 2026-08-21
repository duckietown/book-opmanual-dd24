```{seo}
:description: Troubleshoot common Duckiedrone DD24-B build issues, including power, Raspberry Pi, camera, flight controller, and flight stability problems.
:keywords: Duckiedrone troubleshooting, DD24 power issues, Raspberry Pi boot failure, flight controller debugging, camera not working, drone flight stability, motor issues
```

(dd24-build_troubleshooting)=
# Troubleshooting

It is not uncommon for something to go wrong during the build process, which is why we need to learn how to debug problems. Having a systematic approach is essential to any robotics project.

When encountering a problem, rather than simply redoing the build or replacing a part, try to identify which parts are working and which are not. The Duckiedrone will not fly until _everything_ works.

## Power issues

```{trouble}

My Raspberry Pi does not boot.
---

You should verify that each part of the Duckiedrone is receiving power. The
Raspberry Pi indicates it has power with a *red* power LED.

If your Raspberry Pi is not powering on, verify with a multimeter that the Raspberry Pi
pins are receiving the right voltage on input. You can find a mapping
of the GPIO pins [here](https://www.raspberrypi.org/documentation/usage/gpio/).
Verify that each power pin is receiving 5 volts compared to each ground pin with the multimeter.

Make sure you did not use metal screws to mount the camera to the frame, as they can cause a short.

If your Raspberry Pi is receiving 5 volts on its power/ground pins, but no red
light turns on, then it might have been fried. This can happen if
you miswire or short the power/ground pins on it, so try replacing
the Raspberry Pi.
```

```{trouble}
The motors do not turn on.
---
The motors indicate they are receiving power by beeping once. You can also check each
part with the multimeter. Verify that there is a 12-volt connection
between power and ground on the power distribution board. Also verify
that the Raspberry Pi is receiving 5 volts from the UBEC.
```

## Raspberry Pi Issues

Most of the next debugging steps require getting into your Raspberry Pi using `ssh`. Check the [](first_connection) chapter if you are not connected.

```{trouble}
My Raspberry Pi is receiving power and turning the red light on, but it doesn't boot.

---
Something might be wrong with your microSD card.

*   Verify that your microSD card has the correct image flashed on it.

*   Check that the microSD card is inserted in the Raspberry Pi so that it can boot.

If none of this works, connect a keyboard and monitor to the Raspberry Pi during boot to see what is happening.

There may be an error message being printed on the screen that will give more information.
```

## Camera

For current camera diagnostics, including camera-cable checks and Dashboard symptoms, see [](dd24-troubleshooting-faq). The DD24-B uses the `driver-camera` and `ros2-camera` containers rather than GNU Screen and `raspistill`.

## Flight Controller

Finally, check the Flight Controller. When the Flight Controller
connects to the motors, it will make a "low beep, high beep" sound.
So, verify you hear the "do do do" from the motors, indicating they
have power, and then the "low, high", indicating the Flight Controller
can talk to them. If that doesn't work, check the connection between
the Flight Controller, ESCs, and motors.

Calibrate the accelerometer as described in [](dd24-sensor-calibration). For PX4 and MAVROS flight-controller connection diagnostics, see [](dd24-troubleshooting-faq). The DD24-B uses PX4, so Betaflight configuration does not apply to this workflow.

## Flight Issues

Before each flight, physically inspect the Duckiedrone.

Make sure that:

* your camera is mounted firmly, pointed downwards.
* the range sensor is pointed downwards and has not been rotated.
* the Flight Controller board is level and firmly attached; otherwise, the IMU and gyroscope will return incorrect readings.
* each propeller is tightened down all the way.

Any of these issues could cause poor flight behavior.

If your Duckiedrone flips the first time you try to take off, the motors are
spinning the wrong way, or the props are installed upside down. If your
Duckiedrone makes funny noises when arming, either the props are not
tightened all the way, or they are striking a wire. Tape everything
down as much as possible.

If the Duckiedrone is not stable during flight, you should make sure that
the props are all tightened down. Make sure the ESCs have been
flashed with Bluejay as described in [](dd24-esc-init).

A well-tuned Duckiedrone can hover at zero velocity with some drifting,
but not too much. It should be able to hover with position
hold indefinitely.
