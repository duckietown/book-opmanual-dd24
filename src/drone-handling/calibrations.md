```{seo}
:description: Learn how to calibrate the PX4 IMU and camera on the Duckiedrone (DD24) for stable flight using the Dashboard, ROS 2 services, or direct MAVLink.
:keywords: Duckiedrone accelerometer calibration, Duckiedrone camera calibration, PX4 calibration, IMU calibration, flight controller calibration, DD24 setup
```

(dd24-sensor-calibration)=
# Sensor Calibration

Sensors produce data. To help make sense of this data and transform it into information, it is useful to calibrate sensors.

## PX4 IMU calibration

The IMU is embedded in the flight controller and includes a gyroscope and an accelerometer, which respectively produce angular rate (in degrees/second) and linear acceleration measurements (normalized to gravitational acceleration at sea level $g = 9.81 \frac{m}{s^2}$).

Calibrating the gyroscope and accelerometer gives PX4 the sensor offsets it needs to pass preflight checks and stabilize the drone during flight.

## Method 1: calibration through the Dashboard

On DD24 drones running the Ente ROS 2 stack, the Dashboard talks to PX4 through MAVROS2 and does not require QGroundControl.

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
4. Find the `IMU - Orientation` block.
5. Place the drone still on a level surface and click {bdg-secondary-line}`GYRO`.
6. When gyro calibration completes, click {bdg-secondary-line}`ACCEL`.
7. Follow the live `[cal]` prompts shown in the block. PX4 will ask for six stable orientations: level/top-up, on-back/top-down, nose-down, nose-up, left-side-down, and right-side-down. Hold each orientation still until PX4 accepts it.

The calibration is complete when the block reports `PX4 accel calibration complete` or an equivalent PX4 completion message.

## Method 2: ROS 2 service fallback

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

## Method 3: manual Python fallback on the Raspberry Pi

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

If the flight controller appears on a different device, replace `/dev/ttyACM0` with the correct path. The script sends a ground-control-station heartbeat while it runs, so PX4 emits the `[cal]` status prompts over MAVLink.

(camera-calib)=
## Camera calibration

Follow the camera calibration procedure described [in the Duckiebot opmanual](book-opmanual-duckiebot:camera-calib), making sure to select the tab `Duckiedrone` if you need to print a new calibration pattern.

## Troubleshooting

If the Dashboard, ROS 2 service, and manual Python methods all fail, calibrate the accelerometer from QGroundControl using the PX4 procedure: [PX4 Accelerometer Calibration](https://docs.px4.io/v1.16/en/config/accelerometer).

Use this only as a recovery path. Close QGroundControl before returning to the Duckiedrone stacks, so it does not keep the MAVLink connection open.
