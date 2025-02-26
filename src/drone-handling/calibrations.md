```{seo}
:description: Learn how to calibrate the accelerometer and camera on the Duckiedrone (DD24) for stable flight using Betaflight and the Dashboard.
:keywords: Duckiedrone accelerometer calibration, Duckiedrone camera calibration, Betaflight calibration, IMU calibration, flight controller calibration, DD24 setup
```

(dd24-sensor-calibration)=
# Sensor Calibration

Sensors produce data. To help make sense of this data and transform it into information, it is useful to calibrate sensors.

## Accelerometer calibrations

The IMU is embedded in the flight controller and includes a gyroscope and an accelerometer, which respectively produce angular rate (in degrees/second) and linear acceleration measurements (normalized to gravitational acceleration at sea level $g = 9.81 \frac{m}{s^2}$).

```{figure} ../_images/calibrations/sensors-readings-page.png

{bdg-secondary}`Sensors` tab in Betaflight Configurator showing Gyroscope and Accelerometer readings
```

Calibrating the accelerometer provides the “zero” plane around which the Flight Controller will stabilize the drone during flight, and is a procedure that should be repeated before each flight.

## Method 1: calibration through Betaflight

1.  Connect the Flight Controller to the base station using the micro USB cable.
    
2.  Navigate to the {bdg-secondary}`Setup` page
    
3.  Place the drone on a (very) flat surface
    
    ```{attention}
    Make sure the drone is level on the flat surface.
    ```
        
4.  Click on {bdg-success-line}`Calibrate Accelerometer`
    
    ```{warning}
    Do not move the drone while the calibration is in progress or it will invalidate the calibration.
    ```
        
5.  Wait for a few seconds. A successful outcome will produce a “reset” of pitch and roll measurements and the rendering of the drone’s attitude will show a level condition. 
    

```{figure} ../_images/calibrations/drone-IMU-calibration.gif

Calibration procedure
```

Moreover, a message will appear on the top left of the Betaflight interface saying `Accelerometer calibration finished`.

```{image} ../_images/calibrations/msg_calibration_finished.png
```

## Method 2: calibration through the Dashboard

  
The same result as above can be obtained by connecting to the Dashboard instead of directly hard-wiring the Flight Controller to the base station.  

1.  Connect to the Dashboard ([](first_connection))
    
2.  Navigate to the {bdg-warning}`Robot` > {bdg-dark-line}`Mission Control` page
    
3.  Find the `IMU - Orientation` mission control block
    
4.  In the top right corner of this block, find and click on the {bdg-secondary-line}`Calibrate IMU` button.
    
    * The data stream will freeze for a few seconds while the calibration is undergoing, and it will resume once the calibration is complete.
        
```{figure} ../_images/calibrations/drone-IMU-calibration-dashboard.gif

Calibration of the IMU through the Duckiedrone dashboard
```

## Resetting Yaw offset
The IMU calibration "resets" roll and pitch values to zero, but not the yaw.

```{note}
This part is optional and not strictly needed to have stable flight.
```

Zeroing the yaw defines the new “forward” direction of the drone, and it can be done through the Betaflight interface.

1.  Connect the Flight Controller
    
2.  Navigate to the {bdg-secondary}`Setup` page
    
3.  Click on {bdg-secondary-line}`Reset Z axis, offset`
    
4.  A successful outcome will:
    1. show a new `offset` value on the reset button itself
    2. the rendering will show the drone oriented towards the new zero.
    
```{figure} ../_images/calibrations/drone-YAW-calibration.gif

Drone yaw calibration procedure
```
(camera-calib)=
## Camera calibration

Follow the camera calibration procedure described [in the Duckiebot opmanual](book-opmanual-duckiebot:camera-calib), making sure to select the tab `Duckiedrone` if you need to print a new calibration pattern.