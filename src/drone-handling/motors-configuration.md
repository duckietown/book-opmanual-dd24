```{seo}
:description: Configure the motor order and spinning direction of your Duckietown Duckiedrone (DD24) in QGroundControl, and reverse motors with PX4 dshot.
:keywords: Duckiedrone, DD24, motor order, motor direction, spin direction, QGroundControl, PX4, dshot, Bluejay, Mamba F405 MK2
```

```{needget}
* A base station computer with QGroundControl installed
* An initialized Flight Controller running PX4 with the parameters already loaded
* ESCs already flashed with Bluejay
* The drone battery and a long USB to USB-C cable
---
* A Flight Controller that drives the four motors in the correct order, each turning the right way
```

(dd24-motor-configuration)=
# Configure the motor order and spinning direction

PX4 needs to know which motor sits where on the frame, and every motor needs to turn in the direction PX4 expects. If either of these is wrong the drone will not fly, and it can flip over the instant it leaves the ground, so work through this page slowly.

```{attention}
Connect the battery for this whole page, since the motors need power to spin. Keep the propellers off the entire time.
```

## 1. Connect to the Flight Controller

Connect a long USB to USB-C cable from your computer to the flight controller and open QGroundControl. It connects to the vehicle automatically over USB, the same way as in the [flight controller initialization](qgroundcontrol-connection). Wait until the top toolbar shows the vehicle as connected before you carry on.

## 2. Motor Order

Open the **Actuators** page from the left tab in QGroundControl. This page is where PX4 maps each physical motor on the frame to an output on the flight controller.

```{todo}
Placeholder image: the **Actuators** page as it looks when you first open it, before the motors are assigned.
```

PX4 has a fixed idea of where each motor belongs on a quad. When it refers to Motor 1 it means a specific corner of the frame, not the first wire you happened to solder. The DD24 frame is a quad X, and the numbering PX4 expects is:

1. Motor 1 is at the front right.
2. Motor 2 is at the rear left.
3. Motor 3 is at the front left.
4. Motor 4 is at the rear right.

Front is the direction the drone flies forward, which matches the arrow printed on the frame.

**Identify and label each motor.** QGroundControl can walk you through this. On the **Actuators** page start the motor identification routine. It spins one motor at a time and asks you which corner of the frame just moved. As you answer, QGroundControl assigns that motor to the correct number.

```{todo}
Placeholder image: the motor identification popup in QGroundControl.
```

Sometimes the identification popup does not appear. If that happens you can do the same thing by hand. Turn on the switch that enables the motor sliders on the **Actuators** page, then move one slider by a small amount and watch which motor on the frame spins. Note the corner that moved and set that slider to the matching motor number in the interface. Repeat for all four motors until every number lines up with the real position on the frame.

```{tip}
Move the sliders only a little and keep the propellers off. You need just enough movement to see which motor turns.
```

## 3. Motor Spin Direction

Once every motor is on the correct number, check which way each one turns. On the DD24 the props in layout expects:

1. Motor 1 at the front right turns counter clockwise.
2. Motor 2 at the rear left turns counter clockwise.
3. Motor 3 at the front left turns clockwise.
4. Motor 4 at the rear right turns clockwise.

Spin each motor from the **Actuators** page and compare its direction against the list above. Make a note of any motor that turns the wrong way.

```{note}
Reversing a motor only works when its ESC is running Bluejay, which you flashed in [Initializing the ESCs](dd24-esc-init). On the stock ESC firmware these commands do nothing.
```

To flip a motor you send a command to its ESC over PX4 dshot. Open the terminal inside QGroundControl by going to **Analyze Tools** and then **MAVLink Console**. This drops you into the PX4 shell.

```{todo}
Placeholder image: the **MAVLink Console** entry under Analyze Tools.
```

In the shell, reverse a motor by its number. For example, to reverse Motor 3 you run:

```sh
dshot reverse -m 3
```

Go back to the **Actuators** page and spin that motor again to confirm it now turns the right way.

```{todo}
Placeholder image: the motor turning in the correct direction after the reverse.
```

When the direction is correct, save it so it survives a reboot:

```sh
dshot save -m 3
```

Finally, disconnect and reconnect the battery to power cycle the ESC. Once it comes back the new direction is locked in. Repeat the reverse and save for every motor that was turning the wrong way, then spin all four one last time to confirm they match the layout above.
