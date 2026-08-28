```{seo}
:description: Learn about the Duckiedrone DD24-B software architecture, including Docker containers, DTPS, ROS 2 bridges, PX4, and hardware interfaces.
:keywords: Duckiedrone software architecture, DD24-B containers, DTPS, ROS 2, PX4, MAVROS, camera, ToF sensors
```

(dd24-sw-architecture-autonomy)=
# Software Architecture

The Duckiedrone DD24-B flight stack runs in Docker containers. Sensor drivers publish data through `dtps`, and ROS 2 bridge containers make the required sensor data and flight-controller services available to ROS 2. PX4 runs on the physical flight controller and performs the vehicle estimation and control.

```{note}
The architecture diagram formerly shown here describes the legacy ROS 1/core stack and does not represent the current Duckiedrone DD24-B deployment. See [](duckiedrone-containers) for the current container inventory.
```

## Hardware interfaces

Driver containers interface with hardware and publish their data through `dtps`. Companion ROS 2 bridge containers republish the data for ROS 2 consumers.

### Camera driver

`driver-camera` captures camera frames and publishes them through `dtps`. `ros2-camera` republishes the camera feed as a ROS 2 topic.

### Flight Controller driver

On a physical Duckiedrone, PX4 runs on the flight controller. The `ros2-mavros` container bridges PX4 to ROS 2, exposing flight-controller state and telemetry, accepting commands, and handling arming requests. The base Duckiedrone stack also declares `dt-px4` and `mavlink-proxy`; `dt-px4` provides a software-in-the-loop PX4 simulation only for virtual Duckiedrones, while the role of `mavlink-proxy` depends on the deployed robot configuration.

### ToF driver

`driver-tof-bottom` reads the downward-facing Time-of-Flight sensor and publishes its range measurement through `dtps`. `ros2-tof-bottom` republishes that reading to ROS 2. The downward-facing sensor is required for the standard flight stack; additional ToF sensors are optional.

## Core flight functionality

PX4 runs the core estimator and control loops on the flight controller. The companion-computer containers provide sensor data, ROS 2 accessibility, and Dashboard communications; they do not replace PX4's low-level flight control.

```{note}
The node names in the following subsections come from older stack manifests. `ros1/duckiedrone` declares `fly_commands_mux`, while `core/duckiedrone` declares `pid-controller`, `state-estimator`, and `visual-odometry`. The current Duckiedrone updater selects the ROS 2 stack by default, so it does not select these legacy stacks.
```

### IMU Node

PX4 reads the flight controller's IMU and uses it to estimate attitude. `ros2-mavros` exposes flight-controller IMU telemetry to ROS 2. In the standard Duckiedrone DD24-B configuration, PX4 uses the IMU for attitude estimation but does not fuse GPS, external vision, or optical flow.

### Flight Controller Node

The current stack has no separate `flight_controller_node`. `ros2-mavros` connects PX4 with ROS 2, reports vehicle state and telemetry, receives manual or external-control input, and provides arming and mode-control services. The Dashboard communicates with ROS 2 through `ros2-rosbridge-websocket`.

### ToF Node

The current stack uses `driver-tof-bottom` and `ros2-tof-bottom`. PX4's EKF2 uses the downward-facing ToF sensor as its configured height source; it provides altitude information but no horizontal position estimate.

### FlyCommandsMux Node

`fly_commands_mux` is declared by the legacy `ros1/duckiedrone` stack, not by the current default update stacks. In the current manual workflow, control passes from the Dashboard's Remote Control widget through MAVROS; an external controller in `OFFBOARD` mode publishes its own supported MAVROS setpoint stream and must meet PX4's offboard-control requirements.

### rosbridge

`ros2-rosbridge-websocket` allows the web Dashboard to communicate with ROS 2 through a WebSocket interface.

## Autonomous flight functionality

The default Duckiedrone update starts the `robot/basics`, `duckietown/duckiedrone`, and `ros2/duckiedrone` stacks. These stacks do not declare an autonomous horizontal position-control node. A separate `ros2-core/duckiedrone` stack defines an `altitude` container, but it is not part of the default update stack set. The supplied PX4 configuration disables GPS, external vision, and optical-flow fusion, so it does not provide a standard autonomous horizontal position-control pipeline. PX4 `OFFBOARD` mode is available only when a custom controller supplies compatible setpoints and the estimates required by those setpoints.

### PID

The legacy `core/duckiedrone` stack declares a `pid-controller` service. PX4's built-in control loops manage attitude, yaw rate, and altitude in the current standard stack. The manual does not publish a validated separate PID-controller-node workflow for the Duckiedrone DD24-B; use the supplied PX4 configuration and the guidance in [](dd24-pid-control).

### State Estimator

The legacy `core/duckiedrone` stack declares a `state-estimator` service. PX4's EKF2 provides the state estimate used by the flight controller in the current standard stack. It uses the downward-facing ToF sensor for height and does not fuse GPS, external vision, or optical flow, so it must not be treated as a source of horizontal position or velocity estimates.

### Optical Flow

The legacy `core/duckiedrone` stack declares a `visual-odometry` service. The current camera stack streams imagery for the Dashboard but does not deploy an optical-flow node for flight control. The supplied PX4 configuration disables optical-flow fusion.

### Rigid Transform

Neither the legacy ROS 1/core manifests nor the current default stacks declare a separate Rigid Transform service. The camera is not used to produce a horizontal position estimate in the current standard stack.

### Topics naming

The current ROS 2 topic set comes from the containers running on the Duckiedrone. Inspect it from a ROS 2 environment before relying on a topic name:

```bash
ros2 topic list
ros2 topic echo /mavros/state
```

<!-- markdownlint-disable MD024 -->

#### Flight Controller Node

The legacy `~/flight_controller_node/...` topics and services are not part of the current Duckiedrone DD24-B deployment. Use `ros2-mavros` to inspect PX4 state, telemetry, arming, and mode-control interfaces. `/mavros/state` is the primary flight-controller state topic; depending on the deployed MAVROS plugins, telemetry may also include `/mavros/imu/data`, `/mavros/battery`, and `/mavros/altitude`.

#### IMU Node

The current stack has no separate `imu_node`. IMU telemetry is exposed through MAVROS when its plugin is available. Confirm the topic with `ros2 topic list` before subscribing.

#### ToF Node

The current stack has no `bottom_tof_driver_node`. `ros2-tof-bottom` republishes the downward-facing ToF reading to ROS 2; inspect the running topic list instead of using the legacy `~/bottom_tof_driver_node/range` name.

<!-- markdownlint-enable MD024 -->
