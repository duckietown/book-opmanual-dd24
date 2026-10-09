```{seo}
:description: Learn how to initialize and configure the Duckiedrone Flight Controller for the first time.
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

The Flight Controller handles safety-critical low-level behaviors, such as attitude stabilization. Correct Flight Controller setup is essential for safe flight.

The Duckiedrone DD24-B runs the [PX4 Autopilot](https://px4.io/) firmware, an open-source flight-control software platform, built for the `mamba-f405-mk2` target.

(dd24-fc-setup-steps)=
## Flight Controller setup steps

For a new or re-flashed Flight Controller, first initialize it and then configure it. Repeat either procedure only when the firmware or parameter set is intentionally reinstalled.

```{tableofcontents}
```
