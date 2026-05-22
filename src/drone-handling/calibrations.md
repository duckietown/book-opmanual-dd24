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

### PX4 gyro and accelerometer calibration

On DD24 drones running the Ente ROS 2 stack, the dashboard also provides a `PX4 IMU Calibration` block in Mission Control. This block talks to PX4 through MAVROS2 and does not require QGroundControl.

```{attention}
Remove the propellers before running any flight-controller calibration or arming checks.
```

1. Connect to the Dashboard ([](first_connection)).
2. Start the Duckiedrone stacks if they are not already running:
   ```bash
   dts stack up -H ROBOT_NAME.local duckietown/duckiedrone -d
   dts stack up -H ROBOT_NAME.local ros2/duckiedrone -d
   ```
3. Navigate to {bdg-warning}`Robot` > {bdg-dark-line}`Mission Control`.
4. Find the `PX4 IMU Calibration` block.
5. Place the drone still on a level surface and click {bdg-secondary-line}`GYRO`.
6. When gyro calibration completes, click {bdg-secondary-line}`ACCEL`.
7. Follow the live `[cal]` prompts shown in the block. PX4 will ask for six stable orientations: level/top-up, on-back/top-down, nose-down, nose-up, left-side-down, and right-side-down. Hold each orientation still until PX4 accepts it.

The calibration is complete when the block reports `PX4 accel calibration complete`.

## Method 3: ROS 2 service fallback

If the dashboard is unavailable but the ROS 2 stack is running, call the calibration services from a shell connected to the drone:

```bash
ssh duckie@ROBOT_NAME.local
docker exec -it ros2-px4-calibration bash
source /environment.sh
export ROS_DOMAIN_ID=42

ros2 service call /px4_calibration/calibrate_gyro std_srvs/srv/Trigger
ros2 service call /px4_calibration/calibrate_accel std_srvs/srv/Trigger
```

Watch the live orientation prompts in another shell:

```bash
docker exec -it ros2-px4-calibration bash
source /environment.sh
export ROS_DOMAIN_ID=42
ros2 topic echo /px4_calibration/status std_msgs/msg/String
```

The accelerometer service blocks while you rotate the drone through the six orientations.

## Method 4: manual Python fallback on the Raspberry Pi

Use this only when MAVROS2 or rosbridge is not running. From the base station, stop the stacks that may hold the MAVLink port:

```bash
dts stack down -H ROBOT_NAME.local ros2/duckiedrone
dts stack down -H ROBOT_NAME.local duckietown/duckiedrone
```

Then connect to the Raspberry Pi and run the direct MAVLink script:

```bash
ssh duckie@ROBOT_NAME.local
docker run --rm -it --net host --privileged -v /dev:/dev \
  duckietown/dt-ros2-interface:ente-arm64v8 \
  bash -lc 'source /environment.sh && px4_manual_calibration both --port /dev/ttyACM0'
```

If the flight controller appears on a different device, replace `/dev/ttyACM0` with the correct path. The script sends a ground-control-station heartbeat while it runs so PX4 emits the `[cal]` status prompts over MAVLink.

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
