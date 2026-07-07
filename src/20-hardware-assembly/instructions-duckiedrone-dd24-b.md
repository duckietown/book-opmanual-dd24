```{seo}
:description: Interactive step-by-step instructions to assemble a Duckietown Duckiedrone (DD24-B).
:keywords: Duckietown, Duckiedrone, assembly instructions, quadcopter
```

```{needget}
*   An Duckiedrone `DD24-B` box: [get a Duckiedrone](https://get.duckietown.com/products/autonomous-raspberrypi-quadcopter-duckiedrone-dd24)
*   A `DD24` initialized SD card: [](sw-initialization)
---
*   An assembled Duckiedrone model `DD24-B`
```

```{warning}
🚧 This manual describes the _ente_ software version of the latest Duckiedrone build, and is currently work in progress. 🚧 

Pages marked with this warning are actively being worked on.

The stable release to date is the [_daffy_ Duckiedrone operation manual](https://docs.duckietown.com/daffy/opmanual-dd24/intro.html).  
```

```{todo}
Remove this warning when ready to go.  
```

(duckiedrone-dd24-b-assembly-instructions)=
# Duckiedrone `DD24-B` assembly instructions

```{attention}
There is an error in steps 31–37 of the assembly process.  
Please connect your ToF sensors in this order:
- The **bottom** Time-of-Flight (ToF) sensor must be connected to the port labeled `CHL0`.
- The **left** Time-of-Flight (ToF) sensor must be connected to the port labeled `CHL1`.
- The **right** Time-of-Flight (ToF) sensor must be connected to the port labeled `CHL2`.
- The **top** Time-of-Flight (ToF) sensor must be connected to the port labeled `CHL3`.
- The **front** Time-of-Flight (ToF) sensor must be connected to the port labeled `CHL4`.

Please make these adjustments manually while we work on updating the 3D assembly tool.
```

```{todo}
Future CAD Assembly Improvements
- Show how to insert the Yellow Damping Supports in the FC + ESC Boards
- Highlight the pairing of CW / CCW props with motors
- Update CAD of FC + PDB board to avoid confusion
```

````{attention}

The flight controller and PDB included with the `DD24-B` differ slightly from those depicted in the 3D assembly tool.

**Step 2:**
Ensure the PDB is oriented with the white 6-pin connector facing downward, and the silkscreen labels `M1`, `M2`, `M3`, and `M4` facing upward.

```{figure} ../_images/assembly-dd24-B/esc-board-orientation.jpg
:width: 50%
:alt: Duckiedrone DD24-B ESC orientation
:name: esc-board-orientation

ESC board orientation — this side must face upward.
```

**Step 10 and Step 16:**
Refer to the following schematic for the correct soldering connections on the flight controller:

```{figure} ../_images/assembly-dd24-B/flight-controller-connections-dd24-b.jpg
:width: 50%
:alt: Duckiedrone DD24-B flight controller soldering connections
:name: flight-controller-connections-dd24-b

Flight controller soldering connections.
```
````

<iframe src="/assets/webgl/assembly/DD24/?map_location=resources://&map_name=assembly_DD24&start_step=0&stop_step=-1&framerate=30&show_tutorial=True" frameborder="0" allowfullscreen>
</iframe>


<style>
main#main-content {
    max-width: 100%;
}

main#main-content iframe{
    height: 80vh;
    width: 100%;
}
</style>
