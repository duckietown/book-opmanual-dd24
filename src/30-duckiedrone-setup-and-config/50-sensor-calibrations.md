```{seo}
:description: Learn how to calibrate the PX4 gyroscope, accelerometer, and level horizon on the Duckiedrone DD24-B with QGroundControl for stable flight.
:keywords: Duckiedrone accelerometer calibration, Duckiedrone gyroscope calibration, PX4 calibration, IMU calibration, flight controller calibration, DD24 setup, QGroundControl
```

```{needget}
- A base station computer with [QGroundControl installed](qgroundcontrol-installation)

- A Flight Controller with the Duckiedrone parameters loaded: [](dd24-b-fc-config)

- A data-capable USB-A-to-USB-C cable and any required adapter

- A level surface
---
- A Flight Controller with a calibrated gyroscope, accelerometer, and level horizon
```

(dd24-sensor-calibration)=
# Sensor calibration

The Flight Controller estimates the attitude of the Duckiedrone from its gyroscope and accelerometer. The Duckietown parameter file deliberately contains no calibration values, because they belong to one specific board, so these sensors must be calibrated on the actual Flight Controller before the first flight.

```{danger}
Remove the propellers before calibrating the Flight Controller.
```

## Calibrate the sensors

1. Connect QGroundControl to the Flight Controller over USB, as described in [](dd24-b-fc-config-connect).

2. Open the **Sensors** section in QGroundControl.

3. **Gyroscope:** start the gyroscope calibration and leave the Duckiedrone still on a level surface until it completes.

4. **Accelerometer:** start the accelerometer calibration and hold the Duckiedrone in the six different orientations requested by the on-screen prompts.

5. **Level Horizon:** leave the Duckiedrone still on a level surface until this completes.

### Checkpoint ✅

To verify that the calibrations completed:

```{testexpect}
Open the summary page of QGroundControl.
---
The **Sensors** section is green, as in the figure below.
```

```{figure} ../_images/fc-setup/qgc-summary-post-sensors.png
:name: qgc-summary-post-sensors
:width: 100%
:align: center
:alt: QGroundControl summary page with the Sensors section green after calibration

QGroundControl summary page after the sensor calibrations complete.
```

## Recalibrating later

The gyroscope and level-horizon calibrations can be repeated later without QGroundControl, with the **GYRO** and **LEVEL** buttons of the **IMU - Orientation** widget on the Dashboard: see [](dd24-dashboard-overview).

## Next step

Continue to [](dd24-motor-configuration).
