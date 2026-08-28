```{seo}
:description: Reference for the current and legacy Duckiedrone DD24-B Docker containers, their stacks, and how the standard updater deploys them.
:keywords: duckiedrone, containers, docker, stacks, tof, sensors, dts
```

(duckiedrone-containers)=
# Duckiedrone Containers

The Duckiedrone software is not a single monolithic program. It is a set of smaller programs, each doing one job and handing its result to the next.

Each of those programs is delivered as a Docker **image**, and a running copy of an image is called a **container**, so the software on a Duckiedrone is a set of containers rather than an installed application. This modular architecture and reliance on containerization improve reproducibility.

Containers are grouped into Docker Compose files called **stacks**. A newly flashed Raspberry Pi image provisions `robot/basics`. The standard `dts duckiebot update ROBOT_NAME` command then runs `stack up` for `robot/basics`, `duckietown/duckiedrone`, and `ros2/duckiedrone`. The remaining stacks are optional, separate, or legacy.

To see what is running, log into the Duckiedrone and list the containers:

```bash
ssh duckie@ROBOT_NAME.local
docker ps
```

After a standard update, this list normally includes the containers from the three default stacks, plus containers from any separately started stacks.

## Stacks

| Stack | Contents | Deployment |
| :--- | :--- | :--- |
| `robot/basics` | Container management and the key-value store | Fresh image and standard update |
| `duckietown/duckiedrone` | Message backbone, sensor drivers, simulation support, device services | Standard update |
| `ros2/duckiedrone` | ROS 2 transport and bridges | Standard update |
| `ros2-core/duckiedrone` | Altitude functionality | Separate stack; not selected by the standard update |
| `duckiedrone/extra_sensors` | The front, left, right and top ToF sensors | On request only |
| `duckiedrone/extra_tools` | Tools for inspecting the Duckiedrone | On request only |
| `ros1/duckiedrone` | Legacy ROS 1 interface, rosbridge, and command multiplexer | Legacy; not selected by the current updater |
| `core/duckiedrone` | Legacy PID, state-estimation, and visual-odometry services | Legacy; not selected by the current updater |

The containers started by each stack are grouped below:

```bash
robot/basics
  portainer
  kvstore

duckietown/duckiedrone
  dtps
  driver-camera
  driver-tof-bottom
  dt-px4
  mavlink-proxy
  dashboard
  files-api
  code-api
  device-proxy
  device-health
  device-online
  wifi-access-point

ros2/duckiedrone
  zenoh-router
  ros2-camera
  ros2-tof-bottom
  ros2-mavros
  ros2-px4-calibration
  ros2-rosbridge-websocket

ros2-core/duckiedrone (separate, not selected by the standard update)
  altitude

duckiedrone/extra_sensors (on request only)
  driver-tof-front
  driver-tof-left
  driver-tof-right
  driver-tof-top
  ros2-tof-front
  ros2-tof-left
  ros2-tof-right
  ros2-tof-top

duckiedrone/extra_tools (on request only)
  ros2-foxglove-bridge

ros1/duckiedrone (legacy)
  ros-interface
  ros1-rosbridge-websocket
  fly_commands_mux

core/duckiedrone (legacy)
  pid-controller
  state-estimator
  visual-odometry
```

Run the following stack-management commands from a base station with the Duckietown Shell. They manage the Duckiedrone through its remote Docker endpoint. To bring one up:

```bash
dts stack up -H ROBOT_NAME -d duckiedrone/extra_sensors
```

To take the same stack down again:

```bash
dts stack down -H ROBOT_NAME duckiedrone/extra_sensors
```

The `-H` flag names the Duckiedrone and `-d` detaches, so the command returns instead of streaming the logs of every container.

## Message backbone

Two containers provide the message backbones used by the current stacks.

| Container | Role |
| :--- | :--- |
| `dtps` | A switchboard used by drivers and other DTPS-enabled containers to exchange data |
| `zenoh-router` | Connects the ROS 2 containers. The current Duckiedrone ROS 2 containers use `ROS_DOMAIN_ID=42` to communicate through this router |

## Sensor drivers

Driver containers read the Duckiedrone's sensors. Each one reads a device and publishes its data to the switchboard.

| Container | Role |
| :--- | :--- |
| `driver-camera` | Captures camera data and publishes it through DTPS |
| `driver-tof-bottom` | Reads the downward-facing ToF sensor, which measures height above the ground, and publishes it through DTPS. PX4 uses this sensor as its configured height source |

## ROS 2 bridges

The switchboard is not ROS 2. These containers translate: each one reads from the switchboard and republishes the same data as ROS 2 topics, so that ROS 2 code can use it.

| Container | Role |
| :--- | :--- |
| `ros2-camera` | Republishes the camera feed as a ROS 2 topic |
| `ros2-tof-bottom` | Republishes the downward ToF sensor reading as a ROS 2 topic |
| `ros2-mavros` | Talks to the flight controller and exposes it to ROS 2. Used to read the Duckiedrone's state, send it commands, and arm it |
| `ros2-px4-calibration` | Provides PX4 calibration workflows, including gyroscope and level-horizon calibration |
| `ros2-rosbridge-websocket` | Lets the Dashboard, running in a browser, talk to ROS 2 |

## Simulation

The base stack declares two simulation-related containers. `dt-px4` is explicitly for virtual Duckiedrones; the role of `mavlink-proxy` depends on the deployed robot configuration.

| Container | Role |
| :--- | :--- |
| `dt-px4` | Runs PX4 in software for a virtual Duckiedrone instead of using a physical flight controller |
| `mavlink-proxy` | Routes MAVLink traffic where the deployed configuration uses it |

## Device services

These containers provide the Duckiedrone's web interfaces and back-end services.

| Container | Role |
| :--- | :--- |
| `dashboard` | Runs the on-board Dashboard, the web interface used to fly and monitor the Duckiedrone |
| `files-api` | Gives other tools access to the device's `/data` directory |
| `code-api` | Handles over-the-air software updates for the device |
| `device-proxy` | Makes the device's other APIs and services reachable from one place |
| `device-health` | Reports on the health of the device |
| `device-online` | Connects the device to the Duckietown cloud |
| `wifi-access-point` | Runs the Duckiedrone's own Wi-Fi network |
| `kvstore` | Stores small pieces of data that other containers need to remember |
| `portainer` | A web interface for managing the containers on the device |

(duckiedrone-optional-sensors)=
## Optional sensors

A Duckiedrone's standard PX4 configuration uses the downward-facing sensor as its height source, so `driver-tof-bottom` and `ros2-tof-bottom` are included in the default stack set. The front, left, right, and top sensors are not included by default and start only when requested.

| Container | Role |
| :--- | :--- |
| `driver-tof-front` | Reads the front-facing ToF sensor and publishes it to the switchboard |
| `driver-tof-left` | Reads the left-facing ToF sensor and publishes it to the switchboard |
| `driver-tof-right` | Reads the right-facing ToF sensor and publishes it to the switchboard |
| `driver-tof-top` | Reads the top-facing ToF sensor and publishes it to the switchboard |
| `ros2-tof-front` | Republishes the front ToF sensor reading as a ROS 2 topic |
| `ros2-tof-left` | Republishes the left ToF sensor reading as a ROS 2 topic |
| `ros2-tof-right` | Republishes the right ToF sensor reading as a ROS 2 topic |
| `ros2-tof-top` | Republishes the top ToF sensor reading as a ROS 2 topic |

To start them:

```bash
dts stack up -H ROBOT_NAME -d duckiedrone/extra_sensors
```

The containers in this stack use Docker's `unless-stopped` restart policy. After creation, they restart after a reboot unless explicitly stopped or the stack is taken down.

To stop them:

```bash
dts stack down -H ROBOT_NAME duckiedrone/extra_sensors
```

(duckiedrone-optional-tools)=
## Optional tools

Some containers are useful while developing or debugging, but neither flying the Duckiedrone nor serving the Dashboard needs them. They are not selected by the standard update and start only when requested.

| Container | Role |
| :--- | :--- |
| `ros2-foxglove-bridge` | Exposes the ROS 2 topics to Foxglove, used to visualize and record flights |

To start them:

```bash
dts stack up -H ROBOT_NAME -d duckiedrone/extra_tools
```

To stop them:

```bash
dts stack down -H ROBOT_NAME duckiedrone/extra_tools
```
