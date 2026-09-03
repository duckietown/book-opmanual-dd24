```{seo}
:description: Initialize a Duckiedrone DD24-B microSD card with Balena Etcher or dts init_sd_card.
:keywords: Duckiedrone, software initialization, SD card, flashing, Duckietown, dts, ente, Raspberry Pi 4, Raspberry Pi 5
```

```{needget}
- A computer (the “base station”) with an internet connection

- For the [complete way](dd24-sw-init-adv): a working Duckietown Shell (`dts`) installation

- A microSD card (`64 GB`, U3, Class 10 recommended), e.g., the one from your Duckiedrone box

- A microSD card reader, e.g., the one from your Duckiedrone box
---
- An initialized Duckiedrone microSD card, ready for first boot
```

(dd24-sw-init)=
# Software Initialization

The Duckiedrone uses a Raspberry Pi as an onboard "companion" computer. It requires a Duckietown-specific operating system, and this section describes two ways to install it:

1. [The "fast" way](dd24-sw-init-fast): simpler, works on any operating system, and supports customization only on Ubuntu. It is appropriate for a single Duckiedrone setup. If you plan to connect multiple Duckiedrones to the same network at the same time, use the advanced initialization procedure.

2. [The "complete" way](dd24-sw-init-adv): requires a Duckietown Shell installation on the base station, but offers full customization. You must use this procedure if you plan to use more than one Duckiedrone on the same network at the same time.

```{note}
The legacy pre-built image for the Raspberry Pi 4 (`dt-amelia-DD24-brown2022-sd-card-*.zip`) is no longer supported on the `ente` distribution. If you used it before, re-flash with one of the procedures linked above.
```
