```{seo}
:description: Understand the reference frames used by ROS 2 and PX4 in the Duckiedrone DD24-B system, including REP 105 and REP 103 conventions and a comparison with legacy Betaflight.
:keywords: Duckiedrone reference frames, ROS 2 REP 105, REP 103, PX4 reference frames, legacy Betaflight reference frames, DD24 coordinate systems, robotics frames
```

(dd24-reference-frames)=
# Reference frames

The Duckiedrone DD24-B uses ROS 2 on the Raspberry Pi and PX4 on the flight controller. Their standard body and local/world reference frames differ, so vectors and attitudes that cross an interface must be expressed in the expected frame. The diagrams below also compare the legacy Betaflight body-axis convention.

## PX4 and the legacy Betaflight convention

PX4 uses a forward-right-down (FRD) body frame: `x` points forward, `y` points right, and `z` points down. Its conventional local/world frame has north-east-down axes: `x` points north, `y` points east, and `z` points down. PX4 uses these FRD and north-east-down conventions on its topics unless a message definition states otherwise.[^px4-docs]

The legacy Betaflight illustration uses the same FRD body-axis orientation. This comparison concerns body axes only; it does not make PX4 and Betaflight interfaces interchangeable.

![PX4 and legacy Betaflight reference frames](../_images/software-architecture/px4-betaflight-ref-frame.png)

## ROS 2

ROS 2 does not enforce a single runtime frame convention. For mobile robots such as the Duckiedrone, [REP 103](https://www.ros.org/reps/rep-0103.html) and [REP 105](https://www.ros.org/reps/rep-0105.html) describe the usual conventions. In a robot body frame such as `base_link`, the preferred orientation is forward-left-up (FLU):

- `x` forward

- `y` left

- `z` up

For local Cartesian frames such as `map` and `odom`, the conventional orientation is east-north-up (ENU): `x` east, `y` north, and `z` up. REP 105 defines the roles and relationships of frames such as `base_link`, `odom`, and `map`.

At the ROS 2/PX4 boundary, a body-frame vector converts from FLU to FRD as `(x, y, z)` to `(x, -y, -z)`. A world-frame vector converts from ENU to PX4's north-east-down convention as `(x, y, z)` to `(y, x, -z)`. Attitude quaternions also require the corresponding frame conversion.[^px4-docs]

The image below compares the two reference-frame conventions.

![Comparison between the reference frames used in ROS 2, PX4, and legacy Betaflight](../_images/software-architecture/px4-vs-ros-ref-frames.png)

[^px4-docs]: [PX4 ROS 2 frame-convention documentation](https://docs.px4.io/main/en/ros2/user_guide#ros-2-px4-frame-conventions)
