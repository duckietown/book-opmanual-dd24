```{seo}
:description: Troubleshooting common issues with the Duckiedrone DD24, including power issues, connectivity problems, motor behavior, and software errors.
:keywords: Duckiedrone, troubleshooting, Raspberry Pi, flight controller, motors, software issues, connectivity, dd24 faq
```

(dd24-troubleshooting-faq)=
# Common issues

```{trouble}
The red light on the Raspberry Pi:

*   is blinking
*   does not turn on

---

The Raspberry Pi is not receiving enough power. 

- Check that the voltage coming out of the UBEC is a constant 5V

- Make sure that the HUT is attached to the Raspberry Pi all the way (there is no gap between the GPIO pins and the HUT pin header).

- Make sure that the `OUTPUT` side of the UBEC is attached to the HUT, and the `INPUT` side is soldered to the PDB

- Make sure that there is not a short between the power and ground rails on the HUT.

- Make sure there are no stray wire hairs that are shorting out the `5V` and the `GND` rails on the HUT

```

```{trouble}
The flight containers are not running on the Duckiedrone.

---

The `ente` stack runs the flight code inside Duckietown containers. From the base station, re-pull and restart them with:

    dts duckiebot update -t duckiedrone --distro=ente -f ROBOT_NAME

You can inspect what is running on the Duckiedrone from the Portainer page at `http://ROBOT_NAME.local:9000`. Make sure the containers `dashboard`, `ros2-mavros`, `mavlink-proxy`, `state-estimator`, `pid-controller`, and `visual-odometry` are all healthy.
```

```{trouble}
One of the Heartbeats Monitor indicators is red on the dashboard.
---
A red heartbeat means the corresponding ROS node has stopped publishing. The widget monitors four heartbeats on `/flight_controller_node/heartbeats/*`:

*   `JOYSTICK` — the base station joystick (or the virtual joystick widget).
*   `ALTITUDE` — the ToF altitude node.
*   `STATE` — the state estimator.
*   `PID` — the PID controller.

Find the container for the red node in Portainer and restart it, or rerun `dts duckiebot update -t duckiedrone --distro=ente -f ROBOT_NAME` from the base station.
```

```{trouble}
The flight controller does not connect (MAVROS state is not publishing).

---

On the Duckiedrone, `ros2-mavros` bridges the flight controller to ROS 2. If `/mavros/state` does not publish:

- make sure the USB cable is plugged between the Raspberry Pi and the flight controller (any of the four USB ports works).

- make sure that the flight controller is lighting up. If it is not, the micro USB port on the flight controller may be broken — try a different cable or port, and if the FC still never lights up it may need replacement.

- from the Duckiedrone shell, check that the mavlink endpoint is reachable: `docker logs dt-px4 | tail` should show `INFO  [commander] Ready for takeoff!` once the FC is connected.
```

```{trouble}
The Camera widget is blank.
---

If the camera image never appears, the issue is typically the physical connection between the camera and the Raspberry Pi:

- make sure that the sunny flap is shut (push on the small silver rectangle on the front of the camera and make sure it is attached firmly).

- make sure that the camera flat cable (FFC) is fully inserted into the camera and into the Raspberry Pi.

- On the Raspberry Pi, make sure the blue side of the FFC is facing towards the USB ports.

- On the camera, make sure the blue side of the FFC is facing up.

- Make sure that there are no holes or rips in the FFC. A crash could have caused a tear, or a hole could have been made when soldering. If so, you will need a new FFC.
```

```{trouble}
The Altitude widget is flat (no reading, or stuck at 0).

---

The altitude comes from the ToF driver. Check the Time-of-Flight widget first: if it also shows no value, the `driver-tof` container is down or the sensor is unplugged. Restart the container from Portainer, and if the reading is still missing, verify the I²C connection to the ToF board.
```

```{trouble}
The dashboard shows all widgets but nothing updates.

---

The dashboard communicates with ROS via rosbridge. If the page loads but the widgets never populate, rosbridge on the Duckiedrone is not reachable. From the base station, try `curl -I http://ROBOT_NAME.local:9090` — it should return `101 Switching Protocols`. If not, restart the Duckiedrone containers.
```

```{trouble}
There is a long delay between when you move the Duckiedrone and when the widgets change.

---

This is typically network latency. If you are running the Duckiedrone in CL (client) mode on a shared network, take some of the other devices offline or switch to AP mode and fly against the Duckiedrone's own access point.
```

```{trouble}
The motors on the Duckiedrone do not spin when armed from the dashboard.

---

First confirm that the Arm / Disarm widget actually reports `ARMED` — if the toggle snaps back off, PX4 rejected the arming request (see the troubleshooting list in [](flying_your_drone)).

If the toggle stays on `ARMED` but the motors are silent, check in QGroundControl (see [](qgroundcontrol-connection)) that:

- the ESC/Motor protocol matches the one in the DD24 parameter file,

- each motor can be spun up individually from the `Motors` test page,

- motor 1 is mapped to the bottom-right motor, and the remaining motors are mapped accordingly.
```

```{trouble}
The Duckiedrone does not get off the ground when commanded to take off.

---

- make sure that the arrows embossed on the propellers are visible from the top of the Duckiedrone,

- make sure that the arrows on the props are pointing in the correct direction,

- remove the propellers, plug the battery back in, and from QGroundControl's motor test tab, spin each motor individually to verify the direction,

- make sure that when you spin up motor 1, the bottom-right motor spins. Do this check for all four motors.
```