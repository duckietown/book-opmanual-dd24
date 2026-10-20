```{seo}
:description: Configure the motor order and spinning direction of a Duckiedrone DD24-B in QGroundControl, and reverse motors with PX4 DShot.
:keywords: Duckiedrone, DD24, motor order, motor direction, spin direction, QGroundControl, PX4, DShot, Bluejay, Mamba F405 MK2 V2
```

```{needget}
- A base station computer with [QGroundControl installed](qgroundcontrol-installation)

- An initialized Flight Controller running PX4 with the parameters already loaded

- ESCs already flashed with Bluejay

- The Duckiedrone battery and a long data-capable USB-A-to-USB-C cable
---
- A Flight Controller that drives the four motors in the correct order, each turning the right way
```

(dd24-motor-configuration)=
# Motor configuration

The Flight Controller needs the motor order and spin direction to match the configured quad-X layout. If either is wrong, the Duckiedrone will not fly and may flip over the instant it leaves the ground. Work through this page slowly.

```{danger}
Keep the propellers off the entire time while following instructions on this page.
```

```{attention}
Make sure the battery is charged, and connect it to the Duckiedrone, since the motors need power to spin.
```

## 1. Connect the Flight Controller to the base station

Connect the Flight Controller to the base station with the data-capable USB-A-to-USB-C cable. An extension or an adapter may be needed, depending on the setup.

Then, on the base station open QGroundControl. After a few seconds it will automatically detect the vehicle, as described in the [Flight Controller connection procedure](dd24-b-fc-config-connect). Wait until the top toolbar shows the vehicle as connected before proceeding.

(dd24-motor-order)=
## 2. Motor order

Open the **Actuators** page from the left sidebar in QGroundControl. This page is where PX4 maps each physical motor on the frame to an output on the Flight Controller.

```{figure} ../_images/motors-esc/qGC-actuators-1.jpg
:align: center
:width: 80%
:alt: QGroundControl actuators page showing the Duckiedrone DD24-B motor configuration
:name: qGC-actuators-1

QGroundControl Actuators page showing motor numbering, positions, spin configuration, and ESC communication protocol.
```

PX4 has a fixed idea of where each motor belongs on a quadcopter. When it refers to Motor 1, ..., 4, it means a specific corner of the frame. The Duckiedrone DD24-B frame is a "quad X", and the numbering PX4 expects is:

1. Motor 1 is at the front right.

2. Motor 2 is at the rear left.

3. Motor 3 is at the front left.

4. Motor 4 is at the rear right.

The front is the direction the Duckiedrone flies forward. It aligns with the arrow in the QGroundControl diagram and with the direction of the camera and duckie on the Duckiedrone.

```{figure} ../_images/motors-esc/dd24-top-view.jpg
:align: center
:width: 80%
:alt: Motor configuration on a Duckiedrone DD24-B
:name: dd24-top-view-motors

Motor configuration on the Duckiedrone.
```

**Identify and label each motor.** QGroundControl has a guided routine for this. On the **Actuators** page, start the motor identification routine. It spins one motor at a time and asks which motor spun. QGroundControl then reassigns the outputs to match the selected order.

```{figure} ../_images/motors-esc/dd24-qGC-motor-auto-id.jpg
:align: center
:width: 60%
:alt: QGroundControl motor identification and assignment automatic procedure instructions popup
:name: dd24-qGC-motor-auto-id

QGroundControl automatic motor-identification procedure.
```

```{note}
The motors will spin only if the Duckiedrone battery is plugged in.
```

## 3. Motor spin direction

Once every motor is on the correct number, check which way each one turns. On the Duckiedrone, the "props-in" motor layout expects:

1. Motor 1: front right, black nut, spins counter-clockwise.

2. Motor 2: rear left, black nut, spins counter-clockwise.

3. Motor 3: front left, red nut, turns clockwise.

4. Motor 4: rear right, red nut, turns clockwise.

Spin each motor from the **Actuators** page and compare its direction against the list above. Make a note of any motor that turns the wrong way.

```{warning}
Before reversing a motor, compare its nut color with the list above. A motor whose nut color does not match its corner was mounted on the wrong arm. Do not reverse it: its propeller nut would then unscrew in flight. Fix the assembly instead, by following [](duckiedrone-dd24-b-assembly-instructions).
```

```{note}
The **Set Spin Direction** controls appear after DShot is selected and the motors are assigned. This procedure assumes the Bluejay-flashed ESCs from [Initializing the ESCs](dd24-esc-init).
```

To reverse a motor, send a PX4 DShot command to its ESC:

1. Open the terminal inside QGroundControl by going to **Analyze Tools** and then **MAVLink Console**. This opens the PX4 shell.

   ```{figure} ../_images/motors-esc/qGC-mavlink-console.jpg
   :align: center
   :width: 80%
   :alt: QGroundControl MAVLink Console
   :name: qGC-mavlink-console

   QGroundControl MAVLink Console.
   ```

2. In the shell, reverse the motor by its number. For example, to reverse Motor 3:

   ```sh
   dshot reverse -m 3
   ```

3. Go back to the **Actuators** page and spin that motor again to confirm that it now turns the right way.

4. When the direction is correct, save it so that it survives a reboot:

   ```sh
   dshot save -m 3
   ```

5. Disconnect and reconnect the battery to power cycle the ESC. Once it comes back the new direction is locked in.

Repeat these steps for every motor that was turning the wrong way.

### Checkpoint ✅

To verify the motor order and the spin directions:

```{testexpect}
With the propellers off and the battery connected, spin each of the four motors from the **Actuators** page.
---
Each motor spins at the corner listed in [](dd24-motor-order), in the direction listed above for its nut color.
```

## Next step

Disconnect the battery. The Duckiedrone setup and configuration is now complete: continue to [](dd24-environment-setup).

## Troubleshooting

```{trouble}
The motor identification popup does not appear.
---
Assign the motors by hand. Turn on the switch that enables the motor sliders on the **Actuators** page, then move one slider by a small amount and watch which motor on the frame spins. Note the corner that moved, then assign that output to the corresponding motor function in the interface. Repeat for all four motors until every number lines up with the real position on the frame.

Move the sliders only a little and keep the propellers off. Just enough movement to see which motor turns is needed.
```

```{trouble}
A setup step does not work as described.
---
See [](dd24-troubleshooting-faq).
```
