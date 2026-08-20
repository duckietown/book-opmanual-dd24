```{seo}
:description: Reference for every Docker container running on a Duckiedrone (DD24-B), the stack each one belongs to, and how to enable the underlying code.
:keywords: duckiedrone, containers, docker, stacks, tof, sensors, dts
```

(duckiedrone-containers)=
# Duckiedrone Containers

The Duckiedrone software is not one single, monolithic, program. It is a set of smaller programs, each doing one job and handing its result to the next. 

Each of those programs is delivered as a Docker **image**, and a running copy of an image is called a **container**, so the software on a Duckiedrone is a set of containers rather than an installed application. This modular software architecture design and reliance on containerization technology improves reproducibility of outcomes. 

Containers are grouped into Docker Compose files called **stacks**. Three stacks start automatically at boot up and include everything required for the Duckiedrone to fly. The rest hold optional sensors and tools, and start only when asked.

To see what is running, log into the Duckiedrone and list the containers:

```bash
ssh duckie@ROBOT_NAME.local
docker ps
```

Every container from the three automatic stacks should appear in that list.

## Stacks

| Stack | Contents | Starts |
| :--- | :--- | :--- |
| `robot/basics` | Container management and the key-value store | Automatically |
| `duckietown/duckiedrone` | Message backbone, sensor drivers, flight control, device services | Automatically |
| `ros2/duckiedrone` | ROS 2 transport and the ROS 2 bridges | Automatically |
| `duckiedrone/extra_sensors` | The front, left, right and top ToF sensors | On request only |
| `duckiedrone/extra_tools` | Tools for inspecting the Duckiedrone | On request only |

The containers each stack starts, grouped by stack:

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
```

Stacks are started and stopped from the computer running the Duckietown Shell, not from the Duckiedrone itself. To bring one up:

```bash
dts stack up -H ROBOT_NAME -d duckiedrone/extra_sensors
```

To take the same stack down again:

```bash
dts stack down -H ROBOT_NAME duckiedrone/extra_sensors
```

The `-H` flag names the Duckiedrone and `-d` detaches, so the command returns instead of streaming the logs of every container.

## Message backbone

Two containers carry data between all the others, and everything else depends on them.

| Container | Role |
| :--- | :--- |
| `dtps` | The switchboard. Drivers publish their readings into it and everything else reads them out |
| `zenoh-router` | Connects the ROS 2 containers so they can find and talk to each other. Every ROS 2 container is set to the same `ROS_DOMAIN_ID`, 42, so that they all connect through this router |

## Sensor drivers

Driver containers read the Duckiedrone's sensors. Each one reads a device and publishes what it read into the switchboard.

| Container | Role |
| :--- | :--- |
| `driver-camera` | Captures raw frames from the camera, does some basic processing on them, and publishes them for the rest of the system to use |
| `driver-tof-bottom` | Reads the downward-facing ToF sensor, which measures height above the ground. The altitude controller needs this reading to work, so it is the one sensor required for flight |

## ROS 2 bridges

The switchboard is not ROS 2. These containers translate: each one reads from the switchboard and republishes the same data as ROS 2 topics, so that ROS 2 code can use it.

| Container | Role |
| :--- | :--- |
| `ros2-camera` | Republishes the camera feed as a ROS 2 topic |
| `ros2-tof-bottom` | Republishes the downward ToF sensor reading as a ROS 2 topic |
| `ros2-mavros` | Talks to the flight controller and exposes it to ROS 2. Used to read the Duckiedrone's state, send it commands, and arm it |
| `ros2-px4-calibration` | Runs the calibration steps triggered from the Dashboard, such as calibrating the gyroscope or leveling the horizon |
| `ros2-rosbridge-websocket` | Lets the Dashboard, running in a browser, talk to ROS 2 |

## Simulation

Two containers exist for virtual Duckiedrones only. A physical Duckiedrone has a real flight controller board, so on real hardware both containers just sit idle.

| Container | Role |
| :--- | :--- |
| `dt-px4` | The flight controller software, PX4, running on a computer instead of a physical board. It is the virtual Duckiedrone's flight controller and connects to the simulator for its physics |
| `mavlink-proxy` | Carries flight-controller traffic between the simulated flight controller and the rest of the software |

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
| `wifi-access-point` | Runs the Duckiedrone's own WiFi network |
| `kvstore` | Stores small pieces of data that other containers need to remember |
| `portainer` | A web interface for managing the containers on the device |

(duckiedrone-optional-sensors)=
## Optional sensors

A Duckiedrone flies on the downward-facing sensor alone, so `driver-tof-bottom` and `ros2-tof-bottom` are the only ToF containers that start on their own. The front, left, right and top sensors are not needed to fly, so they stay off by default and only start when asked for.

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

Once started, they keep running and come back automatically after a reboot, until stopped.

To stop them:

```bash
dts stack down -H ROBOT_NAME duckiedrone/extra_sensors
```

(duckiedrone-optional-tools)=
## Optional tools

Some containers are useful while developing or debugging, but neither flying the Duckiedrone nor serving the Dashboard needs them. They stay off by default and only start when asked for.

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