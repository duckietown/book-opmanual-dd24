
```{seo}
:description: Learn how to use a multimeter for continuity and DC voltage checks in your Duckiedrone builds, ensuring accurate and safe electrical connections.
:keywords: Duckiedrone, multimeter usage, continuity check, DC voltage check, electronics testing, multimeter guide, Duckietown, circuit troubleshooting
```

(multimeter-tips)=
# How to Use Multimeters

This guide explains how to use multimeters for performing continuity and voltage checks.

A multimeter, also known as a *VOM* (volt-ohm-milliammeter), is an electronic measuring instrument that combines several functions, such as measuring voltage, current, and resistance.

```{seealso}
[This tutorial](https://learn.sparkfun.com/tutorials/how-to-use-a-multimeter/all) provides a general overview of multimeters.
```

```{attention}
Always turn off the multimeter by setting the dial to `OFF` after completing your checks.
```

```{figure} ../_images/multimeters/multimeters.jpg
:alt: A multimeter used for electronics measurements
:width: 80%
:name: fig:multimeter

A multimeter used for electronics measurements
```

(continuity_check)=
## Continuity Check

In electronics, a continuity check tests whether two points in a circuit are electrically connected. Zero resistance between two points indicates a **short**, which can be hazardous as it may cause excessive current flow, potentially damaging the circuit.

```{note}
Multimeter models may differ slightly, but the general instructions are similar.
```

Performing a continuity check is a safe method for identifying shorts without requiring a power source connected to the circuit.

### Steps for a Continuity Check

1. **Select the Continuity Function**  
   - Turn the multimeter dial to the continuity test position. Press the `"FUNC."` button to enable continuity test mode (indicated by a sound wave icon).  

   | Continuity test dial position | Continuity test mode |
   |:-----------------------------:|:---------------------:|
   | ![](../_images/multimeters/multimeter_dial_continuity_inked.jpg) | ![](../_images/multimeters/multimeter_continuity_mode_inked.jpg) |

   - Test the mode by touching the multimeter leads together. A continuous beep confirms the mode is active.

       ```{figure} ../_images/multimeters/multimeter_leads_together_inked.jpg
       :alt: Multimeter leads touching together to test continuity mode
       :width: 60%
       :name: fig:multimeter-leads-test

       Multimeter leads touching together to test continuity mode
       ```

2. **Perform the Continuity Check**  
   - Place the leads on two points of the circuit to test.  

       ```{figure} ../_images/multimeters/multimeter_leads_on_board.jpg
       :alt: Multimeter leads testing a circuit on a breadboard
       :width: 60%
       :name: fig:multimeter-leads-board

       Multimeter leads testing a circuit on a breadboard
       ```

   - If the path is continuous, the screen will display a near-zero value, and the multimeter will emit a continuous beep.

   ```{note}
   Ignore short beeps followed by silence; they are not indicative of a short.
   ```

```{admonition} General Continuity Check Strategy
:class: tip

- Check all positive (`+`) terminals to ensure continuity between pairs.
- Check all negative (`-`) terminals for continuity between pairs.
- Verify that positive (`+`) terminals are **not continuous** with negative (`-`) terminals.
```

```{seealso}
Watch this [continuity check tutorial](https://www.youtube.com/embed/5G622WDZaHg) for a visual guide.
```

(dc_voltage_check)=
## DC Voltage Check

1. **Select the DC Voltage Mode**  
   Set the multimeter dial to DC voltage mode (indicated by `V` with a straight line `-V` or the symbol `⎓`).

   ```{figure} ../_images/multimeters/multimeter_dial_dc_voltage.jpg
   :alt: Multimeter dial set to DC voltage mode
   :width: 60%
   :name: fig:multimeter-dial-dc

   Multimeter dial set to DC voltage mode
   ```

2. **Perform the Voltage Check**  
   - Place the positive (red) lead on a positive (`+`) terminal and the negative (black) lead on a negative (`-`) terminal.  
   - Observe the voltage measurement displayed on the screen.

```{note}
Reversing the leads (red on `-` and black on `+`) will result in a negative reading but will not cause harm.
```

```{seealso}
For a review of circuit basics and Ohm’s Law ($V = IR$), check out this [SparkFun article](https://learn.sparkfun.com/tutorials/voltage-current-resistance-and-ohms-law).
```
