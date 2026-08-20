```{seo}
:description: Configure the motor order and spinning direction of your Duckietown Duckiedrone DD24-B in QGroundControl, and reverse motors with PX4 DShot.
:keywords: Duckiedrone, DD24, motor order, motor direction, spin direction, QGroundControl, PX4, DShot, Bluejay, Mamba F405 MK2
```

```{needget}
* A base station computer with [QGroundControl installed](qgroundcontrol-connection)
* An initialized Flight Controller running PX4 with the parameters already loaded
* ESCs already flashed with Bluejay
* The Duckiedrone battery and a long USB to USB-C cable
---
* A Flight Controller that drives the four motors in the correct order, each turning the right way
```

(dd24-motor-configuration)=
# Motors configuration

The flight controller needs to know which motor type (CW or CCW) sits where on the frame, and every motor needs to turn in the direction PX4 expects. If either of these is wrong the Duckiedrone will not fly, and it can flip over the instant it leaves the ground, so work through this page slowly.

```{warning}
Keep the propellers off the entire time while following instructions on this page.
```

```{attention}
Make sure the battery is charged, and connect it to the Duckiedrone, since the motors need power to spin.
```

## 1. Connect the Flight Controller to the base station

Connect the USB-C cable coming from the flight controller to your base station. You may need to use an extension or an adapter depending on your setup.

Then, on your base station open QGroundControl. After a few seconds it will automatically detect the vehicle, as in the [flight controller initialization](qgroundcontrol-connection) procedure. Wait until the top toolbar shows the vehicle as connected before proceeding.

## 2. Motor Order

Open the **Actuators** page from the left tab in QGroundControl. This page is where PX4 maps each physical motor on the frame to an output on the flight controller.

```{figure} ../_images/motors-esc/qGC-actuators-1.jpg
:width: 80%
:alt: QGroundControl actuators page showing the Duckiedrone DD24-B motor configuration.
:name: qGC-actuators-1

QGroundControl Actuators page, indicating: motor numbering, positioning, spin configuration and ESC communication protocol.
```

PX4 has a fixed idea of where each motor belongs on a quadcopter. When it refers to Motor 1, ..., 4, it means a specific corner of the frame. The DD24(-B) frame is a "quad X", and the numbering PX4 expects is:

1. Motor 1 is at the front right.
2. Motor 2 is at the rear left.
3. Motor 3 is at the front left.
4. Motor 4 is at the rear right.

Front is the direction the Duckiedrone flies forward, which matches the arrow printed on the frame in QGroundControl, and the direction of the camera (and duckie) on the Duckiedrone.

```{figure} ../_images/motors-esc/dd24-top-view.jpg
:width: 80%
:alt: Motor configuration on a Duckiedrone DD24-B
:name: dd24-top-view-motors

Motor configuration on the Duckiedrone.
```

**Identify and label each motor.** QGroundControl can walk you through this. On the **Actuators** page start the motor identification routine. It spins one motor at a time and asks you which corner of the frame just moved. As you answer, QGroundControl assigns that motor to the correct number.

```{figure} ../_images/motors-esc/dd24-qGC-motor-auto-id.jpg
:width: 60%
:alt: QGroundControl motor identification and assignment automatic procedure instructions popup
:name: dd24-qGC-motor-auto-id

Motors automatic identification procedure.
```

Sometimes the identification popup does not appear. If that happens you can do the same thing by hand. Turn on the switch that enables the motor sliders on the **Actuators** page, then move one slider by a small amount and watch which motor on the frame spins. Note the corner that moved and set that slider to the matching motor number in the interface. Repeat for all four motors until every number lines up with the real position on the frame.

```{tip}
Move the sliders only a little and keep the propellers off. You need just enough movement to see which motor turns.
```

```{reminder}
The motors will spin only if the Duckiedrone battery is plugged in.
```

## 3. Motor Spin Direction

Once every motor is on the correct number, check which way each one turns. On the Duckiedrone, the "props-in" motor layout expects:

1. Motor 1: front right, black nut, spins counter-clockwise.
2. Motor 2: rear left, black nut, spins counter-clockwise.
3. Motor 3: front left, red nut, turns clockwise.
4. Motor 4: rear right, red nut, turns clockwise.

Spin each motor from the **Actuators** page and compare its direction against the list above. Make a note of any motor that turns the wrong way.

```{note}
Reversing a motor spinning direction with the "Set Spin Direction 1/2" buttons only works when its ESC is running Bluejay, which was flashed in [Initializing the ESCs](dd24-esc-init). On the stock ESC software, without Bluejay configuration firmware, these commands do nothing.
```

To flip a motor spinning direction, alternatively, send a command to its ESC over PX4 DShot. Open the terminal inside QGroundControl by going to **Analyze Tools** and then **MAVLink Console**. This drops you into the PX4 shell.

```{figure} ../_images/motors-esc/qGC-mavlink-console.jpg
:width: 80%
:alt: QGroundControl MAVLink Console
:name: qGC-mavlink-console

QGroundControl MAVLink Console
```

In the shell, reverse a motor by its number. For example, to reverse Motor 3 you run:

```sh
dshot reverse -m 3
```

Go back to the **Actuators** page and spin that motor again to confirm it now turns the right way.

When the direction is correct, save it so it survives a reboot:

```sh
dshot save -m 3
```

Finally, disconnect and reconnect the battery to power cycle the ESC. Once it comes back the new direction is locked in. Repeat and save for every motor that was turning the wrong way, then spin all four one last time to confirm they match the nominal layout.
