
```{seo}
:description: Learn essential skills for building your Duckiedrone, including wire stripping, tinning, soldering, and troubleshooting techniques.
:keywords: Duckiedrone, soldering guide, wire stripping, tinning wires, drone building, troubleshooting soldering, Duckietown, soldering tips
```

(dd24-prerequisites-soldering)=
# Soldering Basics

This section outlines the most important skills you will need for building your Duckiedrone.

```{warning}
Whether you are new to soldering or not, we recommend reading through this page to review the basic techniques used in the build.
```

## Stripping Wires

Stripping involves removing a portion of insulation from a wire to expose its strands. This is done using a wire stripper. Once exposed, the wire can be tinned and soldered.

## Tinning

Tinning is the process of applying solder to exposed wire or a metal pad. Using a soldering iron, heat the metal and melt the solder onto the surface. Tinning simplifies the soldering process by pre-coating the wire or pad.

### Tinning a Wire

Watch the following tutorial to learn how to tin and join two wires:

<iframe width="560" height="315" src="https://www.youtube.com/embed/pRPF4wpXX9Q" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

```{note}
Some parts may arrive pre-tinned by the manufacturer. You can identify pre-tinned wires by:
1. The "shininess" of the wire tip.
2. The inability to fray the wire strands at the tip.

However, pre-tinned tips are often ineffective. Cut off pre-tinned tips, then strip and tin the wire yourself.
```

(sec:soldering)=
## Soldering

Soldering involves joining two metal components by melting solder, which acts as a conductive bridge for electricity. Below are key safety and technique tips:

```{danger}
- Use **long-nose pliers** or helping hands to hold wires and components, as they become hot quickly during soldering.
- Avoid touching the soldering iron tip or other heated metal parts to prevent burns. If burned, rinse the area immediately with cold water.
- Do not use the soldering iron on unintended surfaces; the heat can cause melting or damage.
- Always use a **fume extractor** to avoid inhaling fumes.
```

### Resources

- For a quick soldering overview, watch [this YouTube tutorial](https://www.youtube.com/watch?v=Qps9woUGkvI).
- For a more detailed introduction, review [this article](https://www.circuitrework.com/guides/7-1-1.html).

(build-troubleshooting)=
## Troubleshooting

```{trouble}
I accidentally cut off too much wire while stripping.
---
Strip and tin one end of a new wire of the same color, then solder it to the original wire. Cover the solder joint with heat shrink tubing or electrical tape.
```

```{trouble}
I cut off several strands of wire while stripping.
---
If only a few strands are missing, the wire is likely still usable. If many strands are gone, replace the wire.
```

```{trouble}
I put too much solder on my wire while tinning.
---
Remove excess solder using a solder sucker or copper wick. Alternatively, use a soldering iron to pick up excess solder and clean it off with brass wool.
```

```{trouble}
My wire picks up random particles while tinning.
---
Clean the soldering iron tip with brass wool frequently during use to prevent contamination.
```

```{trouble}
The alligator clips of my helping hands are loose.
---
Remove the loose clip, pinch the end tighter with pliers, and reattach.
```

```{trouble}
I burned some insulator onto my wire while tinning.
---
Small amounts of insulator burn are fine. For significant damage, carefully cut off the burned section or replace the wire.
```

```{trouble}
I cannot tell if my wire is properly tinned.
---
Cut off the tip of the wire and inspect the core. If the solder has not penetrated the core or the strands can still spread, the wire is not properly tinned.
```

```{trouble}
My solder keeps forming into a sphere instead of sticking to the wire.
---
This issue occurs when the solder is not fully melting due to:
1. A worn-out soldering iron tip.
2. Expired solder.
3. A malfunctioning soldering iron station.
4. Using non-rosin core solder. Apply soldering flux to the wire or try a different solder type.
```

