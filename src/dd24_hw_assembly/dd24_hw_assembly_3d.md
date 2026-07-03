```{seo}
:description: Instructions to build the Duckiedrone model DD24 using a 3D visualizer.
:keywords: Duckiedrone DD24, hardware assembly guide, build dd24, build duckiedrone, build instructions dd24
```

(dd24-hw-assembly)=
# Duckiedrone (`DD24`) - 3D Assembly Tool

We created this "3D Assembly Tool" to help visualize the assembly steps of the `DD24` Duckiedrone in a more intuitive way than 2D instructions.

```{warning}
There is an error in steps 34 and 36–37 of the assembly process.  
- The **bottom** Time-of-Flight (ToF) sensor must be connected to the port labeled `CHL0` (not `CHL2` as currently shown).  
- The **front** Time-of-Flight (ToF) sensor should be connected to `CHL2`.  

Please make these adjustments manually while we work on updating the 3D assembly tool.
```

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