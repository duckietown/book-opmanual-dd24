```{seo}
:description: Understand the reference frames used by ROS 2 and PX4 in the Duckiedrone DD24-B system, including REP 105 and REP 103 conventions and a comparison with legacy Betaflight.
:keywords: Duckiedrone reference frames, ROS 2 REP 105, REP 103, PX4 reference frames, legacy Betaflight reference frames, DD24 coordinate systems, robotics frames
```

(dd24-reference-frames)=
# Reference frames

The Duckiedrone DD24-B uses ROS 2 on the Raspberry Pi and PX4 on the flight controller, which use different reference-frame conventions. The diagrams below also compare the legacy Betaflight convention.

## PX4 and the legacy Betaflight convention

PX4 uses the following reference frame. The diagram includes the legacy Betaflight convention for comparison[^px4-docs]:

![PX4 and legacy Betaflight reference frames](../_images/software-architecture/px4-betaflight-ref-frame.png)

## ROS 2

ROS 2 uses the reference frame conventions defined in [REP 105](https://www.ros.org/reps/rep-0105.html). Specifically for mobile robots (and thus the Duckiedrone), the _preferred_ axis orientation is defined in [REP 103](https://www.ros.org/reps/rep-0103.html) as having the _body axes_ as:

- `x` forward
- `y` left
- `z` up.

In the image below, you can see a comparison between the two reference frame conventions.[^px4-docs]

![Comparison between the reference frames used in ROS 2, PX4, and legacy Betaflight](../_images/software-architecture/px4-vs-ros-ref-frames.png)

[^px4-docs]: [PX4 documentation source](https://docs.px4.io)
