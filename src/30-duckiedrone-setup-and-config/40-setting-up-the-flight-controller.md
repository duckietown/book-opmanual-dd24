```{seo}
:description: Introduction on how to perform the first-time initialization and configuration of the Duckiedrone's flight controller.
:keywords: Duckiedrone, Duckietown, autonomous drone, uav, flight controller, initialization, PX4, dfu-util, mamba-f405-mk2
```

```{needget}
* A base station computer running Linux (Ubuntu) or macOS
* "Mamba" (`DD24-B`) Flight Controller
* USB to USB-C cable with data
---
* An up-to-date, initialized "Mamba" FC running PX4
```

(dd24-fc-setup)=
# Setting up the Flight Controller

The Flight Controller (FC) implements several safety-critical low-level behaviors, e.g., attitude stabilization. Correctly configuring the FC is paramount for achieving safe flight.

The `DD24-B` runs the [PX4 Autopilot](https://px4.io/) firmware, built for the `mamba-f405-mk2` target.

PX4 is a popular, open-source flight control software ultimately directed by the [Linux Foundation](https://www.linuxfoundation.org/).

(dd24-fc-setup-steps)=
## Flight controller setup steps

To set up the flight controller, we will first initialize and then configure it. Both procedures should be performed only once.

```{tableofcontents}
```

