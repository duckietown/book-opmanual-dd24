```{seo}
description: Interactive step-by-step instructions to assemble a Duckietown Duckiedrone (DD24-B).
keywords: Duckietown, Duckiedrone, assembly instructions, quadcopter
```

(duckiedrone-dd24-b-assembly-instructions)=
# Duckiedrone `DD24-B` assembly instructions

```{note}
Before proceeding with these instructions, make sure to [identify the flight controller type in your Duckiedrone box](identifying-flight-controller). 
```

```{warning}
There is an error in steps 34 and 36–37 of the assembly process.  
- The **bottom** Time-of-Flight (ToF) sensor must be connected to the port labeled `CHL0` (not `CHL2` as currently shown).  
- The **front** Time-of-Flight (ToF) sensor should be connected to `CHL2`.  

Please make these adjustments manually while we work on updating the 3D assembly tool.
```

````{attention}

The flight controller and PDB board included with the `DD24-B` differ slightly from those depicted in the 3D assembly tool.

**Step 2:**
Ensure the PDB board is oriented with the white 6-pin connector facing downward, and the silkscreen labels `M1`, `M2`, `M3`, and `M4` facing upward.

```{figure} ../_images/assembly-dd24-B/esc-board-orientation.jpg
:width: 30%

ESC board orientation — this side must face upward.
```

**Step 10 and Step 16:**
Refer to the following schematic for the correct soldering connections on the flight controller:

```{figure} ../_images/assembly-dd24-B/flight-controller-connections.png
:width: 30%

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
