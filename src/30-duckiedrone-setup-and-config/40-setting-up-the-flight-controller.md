```{seo}
:description: Learn how to initialize and configure the Duckiedrone flight controller for the first time.
:keywords: Duckiedrone, Duckietown, autonomous drone, uav, flight controller, initialization, PX4, dfu-util, mamba-f405-mk2
```

```{needget}
- A base station computer running Linux (Ubuntu) or macOS

- "Mamba" (DD24-B) Flight Controller

- A data-capable USB cable and any required adapter to connect the base station to the Flight Controller's USB-C port

- ESCs already flashed with Bluejay
---
- A "Mamba" Flight Controller running PX4 with the Duckietown parameters loaded
```

(dd24-fc-setup)=
# Setting up the Flight Controller

The Flight Controller (FC) handles safety-critical low-level behaviors, such as attitude stabilization. Correct FC setup is essential for safe flight.

The Duckiedrone DD24-B runs the [PX4 Autopilot](https://px4.io/) firmware, built for the `mamba-f405-mk2` target.

PX4 is an open-source flight-control software platform.

(dd24-fc-setup-steps)=
## Flight controller setup steps

For a new or re-flashed Flight Controller, first initialize it and then configure it. Repeat either procedure only when you intentionally reinstall the firmware or parameter set.

```{tableofcontents}
```
