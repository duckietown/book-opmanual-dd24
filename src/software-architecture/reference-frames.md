```{seo}
:description: Understand the different reference frames used by ROS and Betaflight in the Duckiedrone (DD24) system, including REP 105 and REP 103 conventions.
:keywords: Duckiedrone reference frames, ROS REP 105, REP 103, Betaflight reference frame, DD24 coordinate systems, robotics frames, PX4 documentation
```

(dd24-reference-frames)=
# Reference frames

Different reference frames are used by ROS and Betaflight (i.e., by the software running on the Raspberry Pi and the one on the flight controller, respectively).

## Betaflight

The reference frame used by the Betaflight firmware is as follows[^px4-docs]:

![Betaflight Reference Frame](../_images/software-architecture/px4-betaflight-ref-frame.png)

## Robot Operating System (ROS)

ROS uses the reference frame conventions defined in [REP 105](https://www.ros.org/reps/rep-0105.html). Specifically for mobile robots (and thus the Duckiedrone), the _preferred_ axis orientation is defined in [REP 103](https://www.ros.org/reps/rep-0103.html) as having the _body axis_ as:

- `x` forward
- `y` left
- `z` up.

In the image below, you can see a comparison between the two reference frame conventions.[^px4-docs]

![Comparison between the reference frames used in ROS and PX4/Betaflight](../_images/software-architecture/px4-vs-ros-ref-frames.png)

[^px4-docs]: [Source px4-docs](https://docs.px4.io)