```{seo}
:description: Understand PID control on the Duckiedrone DD24-B and configure its PX4 flight controller through QGroundControl.
:keywords: Duckiedrone, DD24-B, flight controller, PID tuning, PX4, QGroundControl, flight stability
```

(betaflight-pid-settings)=
# Flight Controller PID Tuning

```{needget}
* A fully operational Duckiedrone DD24-B
---
* A Duckiedrone ready for a controlled test flight
```

The Duckiedrone DD24-B uses PX4 for flight control. Configure its flight controller with QGroundControl and the Duckietown parameter file as described in [](dd24-b-fc-config).

```{warning}
The Betaflight PID tuning workflow and its recommended values do not apply to DD24-B. Do not use Betaflight Configurator or copy Betaflight-specific PID values onto the PX4 flight controller.
```

## PID control

A proportional-integral-derivative (PID) controller is one part of how the flight controller stabilizes the Duckiedrone. It uses feedback from sensors, including the IMU, to respond to commanded attitude and yaw-rate changes.

In a stable test flight, the Duckiedrone should respond smoothly to roll, pitch, and yaw commands without persistent oscillation or unintended rotation.

## Current DD24-B procedure

1. Load the supplied PX4 parameter file in QGroundControl as described in [](dd24-b-fc-config).
1. Complete the required flight-controller calibrations.
1. Verify that the Duckiedrone behaves as expected in a controlled test flight.

## Further tuning

The manual does not currently publish a validated per-axis PX4 PID-tuning procedure or replacement values for DD24-B. Change individual controller parameters only with a validated project procedure and support from the Duckietown team.
