
```{seo}
:description: Learn about additional materials required for building and flying the DD24 Duckiedrone, including base station requirements and essential tools.
:keywords: Duckiedrone, required materials, soldering tools, base station, Duckietown drone build, autonomous quadcopter tools
```

<!--
```{needget}
- A computer (laptop, or desktop)
---
- Knowledge of best approach to installing Duckietown on your computer
```
-->

(dd24-prerequisites-other)=
# Additional Materials for Building the Duckiedrone

This section details the materials needed to build and fly your drone that are not included in the kit.

## Base Station Computer

To fly the drone, you must have a laptop or personal computer that is capable of connecting to the internet. 

```{todo}
TODO: point to list of minimum hardware requirements after merging this in main manual
```

### OS Requirements

The Duckiedrone ente version requires either:

* Native (or dual boot) Ubuntu 24.xx, or
* macOS

Most functionalities are expected to work with [Duckietown Workspaces](https://docs.duckietown.com/ente/duckietown-manual/10-setup/00-computer/setup-duckietown-workspace.html) on any operating system. 


```{todo}
TODO: update link above after mergin into the Duckietown manual.
```

### Software Requirements

A few programs will be introduced throughout these instructions when needed. 

At this stage, complete the [Duckietown general initial setup](https://docs.duckietown.com/ente/duckietown-manual/10-setup/setup-introduction.html) which will guide you through:

* Setting up your computer, depending on the OS
* Creating useful third-party accounts (e.g., GitHub, DockerHub, Duckietown account)
* Accessing support channels
* Installing the Duckietown Shell
* Installing the Duckiematrix simulation environment

```{todo}
TODO: update link above after mergin into the Duckietown manual.
```

## Essential tools and accessories

The Duckiedrone is designed to require soldering, in the spirit of "when you will brake it, you will know how to fix it". Here is a list of soldering-related items.

```{figure} ../_images/tools/Soldering_iron_and_accessories.jpg
:name: soldering-iron
:width: 300px
:align: center
:alt: Example of soldering iron, sponge and solder

Soldering iron with base, sponge and solder
```

- Soldering iron: capable of reaching a temperature of at least 450-480 degrees Celsius 
- Solder: use only lead-free solder
- Brass wool or sponge to clean soldering iron

```{figure} ../_images/tools/Ideal_Reflex_wire_stripper.jpg
:name: fig-wire-stripper
:width: 300px
:align: center
:alt: Example of wire stripper

Example of wire stripper
```

- Safety goggles: it is good practice to wear them while soldering
- Fume extractor: only solder in a well ventilated environment, or using a dedicated fume extractor
- Helping hands: will dramatically simplify a few soldering passages

## Nice to have tools and accessories

- A chisel or bevel soldering iron tip: will simplify the passages of soldering to pads
- Solder remover: either solder sucker or de-soldering wick, to fix potential mistakes
- Soldering mat: to keep it clean and safe
- Tweezers or pliers
- Wire strippers

## Miscellaneous Tools

- Multimeter: to verify the build (no shorts during soldering, appropriate voltage where needed)
- Joypad: not necessary, but nice to have to manually control the Duckiedrone
- A long (~1m) USB-C to base-station cable **with data**, or, a female USB-A to base station extension cable **with data**: to simplify connecting the flight controller to your computer, for initial configuration. 
