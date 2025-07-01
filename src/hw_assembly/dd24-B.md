# 3D Assembly Tool - DD24-B
The 3D Assembly Tool is a powerful software application designed to assist users in visualizing and assembling the DD24. With its intuitive interface and advanced features, it provides step-by-step assembly instructions.

```{warning}
There is a mistake in Step 34, the bottom time of flight sensor must be plugged in the connector labeled `CHL0` rather than `CHL3`. Make sure to do so while we work on updating the 3D tool.
```

````{attention} Differences in the 3D Assembly Tool

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
