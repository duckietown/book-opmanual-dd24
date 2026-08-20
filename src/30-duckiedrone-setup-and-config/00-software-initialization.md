```{seo}
:description: Flash the Duckiedrone DD24-B ente image onto a Raspberry Pi 4 or Raspberry Pi 5 using dts init_sd_card.
:keywords: Duckiedrone, software initialization, SD card, flashing, Duckietown, dts, ente, Raspberry Pi 4, Raspberry Pi 5
```

```{needget}

* A computer (the “base station”) with an internet connection and the Duckietown Shell (`dts`) installed
* A microSD card (64 GB, U3, Class 10 recommended), e.g., the one from your Duckiedrone box
* A microSD card reader, e.g., the one from your Duckiedrone box
---

* A Duckiedrone initialized and customized microSD card, ready for first boot
```

(dd24-sw-init)=
# Software Initialization

The Duckiedrone's uses a Raspberry Pi as onboard "companion" computer. The Raspberry Pi requires a (Duckietown-specific) operating system to work, and this section details two ways to do so: 

1. [The "fast" way](dd24-sw-init-fast): simpler, but provides no customization options (unless using Ubuntu as OS). It is appropriate for a single Duckiedrone setup. If you are planning to connect multiple Duckiedrones to the same network at any time, use the advanced initialization procedure. 
2. [The "complete" way](dd24-sw-init-adv): requires a Duckietown Shell installation on the base station, but offers full customizability. Must use this procedure if planning to use more than one Duckiedrone on the same network at the same time. 

If using Ubuntu, the best of both worlds can be achieved: [](). 

```{note}
The legacy pre-built image for the Raspberry Pi 4 (`dt-amelia-DD24-brown2022-sd-card-*.zip`) is no longer supported on the `ente` distribution. If you used it before, re-flash with one of the procedures linked above.
```