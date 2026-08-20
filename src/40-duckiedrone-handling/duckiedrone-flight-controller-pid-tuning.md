```{seo}
:description: Learn how to tune the flight controller PID settings on your Duckiedrone for stable and responsive flight.
:keywords: Duckiedrone, flight controller, PID tuning, Betaflight, drone stability
```

(betaflight-pid-settings)=
# Flight Controller PID Tuning

```{needget}
* A fully operational `DD24`
---
* A `DD24` that flies stably and responsively in manual mode
```

When you start up a consumer drone for the first time, you would expect it to take off and fly more or less stably, rather than shaking or going out of control. To get the Duckiedrone to fly, we will have to understand the magic happening in the flight controller, and configure it appropriately. 

In this section, you will learn about the parameters on the flight controller that enable this stability, and you will have the opportunity to adjust them.

## Expected Behavior with Proper PID Settings

The Flight Controller (FC) runs high-frequency control loops to stabilize the Duckiedrone. It utilizes sensors (e.g., IMU and gyroscope) to estimate the Duckiedrone's state. In **Angle Mode**, the following behavior is expected:

- **Roll/Pitch Axis:**
  - When commands are issued, the FC attempts to achieve the commanded roll/pitch as quickly as possible while minimizing fluctuations.
  - When no commands are given, the Duckiedrone will return to a neutral roll and pitch.

- **Yaw Axis:**
  - When commands are issued, the FC maintains the desired yaw rate.
  - When no commands are given, the Duckiedrone should remain stable without unintended rotation.

## PID Control in Drones and Tuning

A **proportional–integral–derivative (PID) controller** is used in the FC to achieve this stability. Below are useful references explaining the influence of `P`, `I`, and `D` terms in drone flight and how they should be tuned:

- [FPV Drone PID Explained by Oscar Liang](https://oscarliang.com/pid/)
- [Betaflight PID Tuning on OpenTXU by John Case](http://open-txu.org/home/special-interests/multirotor/betaflight-pid-tuning/)
- [Video on Recognizing PID Issues](https://www.youtube.com/watch?v=YNzqTGEl2xQ) - Demonstrates typical PID tuning problems.

## Adjusting PID Values in Betaflight

### How to Update PID Settings

```{figure} ../_images/fc-pid-setting/FC_PID_starter.png
:width: 100%
:name: fig-fc-pid-setting-fc-pid-starter
:alt: Screenshot of Betaflight PID tuning interface with recommended starter values

GUI tab in Betaflight to set the PID values (with recommended starter values).
```

To update the PID values in Betaflight:

1. Connect the FC to the base station.
2. Navigate to the "PID Tuning" tab.
3. Adjust the values and click "Save" in the bottom-right corner.

### Recommended Starting Values

| Axis  | P   | I   | D   |
|-------|-----|-----|-----|
| Roll  | 80  | 60  | 75  |
| Pitch | 80  | 60  | 75  |
| Yaw   | 70  | 45  |     |

After entering these values, click `Save`.

### Your PID Tuning Process

```{note}
When adjusting PID settings, only modify `P`, `I`, and `D` for the **Roll**, **Pitch**, and **Yaw** axes. Do not change other values unless you are sure of their impact.
```

To refine the manual flight performance of your Duckiedrone, follow this loop:

1. Fly the Duckiedrone manually and observe any stability issues.
2. Connect the FC to Betaflight.
3. Adjust the PID values accordingly.

Repeat the process until the Duckiedrone exhibits smooth, stable, and responsive behavior.

---

By following this guide, you will optimize your Duckiedrone's flight stability and responsiveness, making it easier to control during manual operation.
