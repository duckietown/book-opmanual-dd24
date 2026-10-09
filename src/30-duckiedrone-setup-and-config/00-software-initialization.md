```{seo}
:description: Initialize a Duckiedrone DD24-B microSD card with Balena Etcher or dts sd_card init.
:keywords: Duckiedrone, software initialization, SD card, flashing, Duckietown, dts, ente, Raspberry Pi 4, Raspberry Pi 5
```

```{needget}
- A computer (the "base station") with an internet connection

- A working Duckietown Shell (`dts`) installation: [Install the Duckietown Shell](https://docs.duckietown.com/ente/duckietown-manual/10-setup/02-software/duckietown-shell-dts-installation.html)

- A microSD card (`64 GB`, U3, Class 10 recommended), e.g., the one from the Duckiedrone box

- A microSD card reader, e.g., the one from the Duckiedrone box
---
- An initialized Duckiedrone microSD card, ready for first boot
```

(dd24-sw-init)=
# Software Initialization

The Duckiedrone uses a Raspberry Pi as an onboard "companion" computer. It requires a Duckietown-specific operating system, and this section describes two ways to install it:

1. [The "fast" way](dd24-sw-init-fast): simpler, works on any operating system, and supports customization only on Ubuntu. It is appropriate for a single Duckiedrone setup. If you plan to connect multiple Duckiedrones to the same network at the same time, use the advanced initialization procedure.

2. [The "complete" way](dd24-sw-init-adv): requires a Duckietown Shell installation on the base station, but offers full customization. You must use this procedure if you plan to use more than one Duckiedrone on the same network at the same time.

After initializing a microSD card and completing the [first boot](dd24-first-boot), settings such as the hostname, Wi-Fi configuration, or `duckie` account password can be changed without reflashing it. Follow the [](dd24-update-initialized-sd-card) instructions for `dts sd_card update`.

```{note}
The legacy pre-built image for the Raspberry Pi 4 (`dt-amelia-DD24-brown2022-sd-card-*.zip`) is no longer supported on the `ente` distribution. If you used it before, re-flash with one of the procedures linked above.
```
