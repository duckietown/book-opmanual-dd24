```{seo}
:description: Description of components inside the Duckiedrone DD24 box.
:keywords: duckietown, duckiedrone, dd24, box, components, what is in the box, drone, DIY drone, autonomous drone, fpv drone, quadcopter
```

(prelim-dd24-box-component-description)=
# Description of Components (`DD24`)

Most components in the Duckiedrone box are functional, i.e., they serve a purpose in learning how to or directly flying the drone. Other components are not functional, but still useful. 

We provide below a brief description of each component in the box.

(component-raspberry-pi-4-4gb)=
## Raspberry Pi 4 - Model B - 4GB

```{figure} ../_images/components-official-dd24/rpi4.jpg
:name: rpi4
:width: 400px
:align: center
:alt: Raspberry Pi 4 Model B, 4GB RAM

Raspberry Pi 4 Model B, 4GB RAM
```

The Raspberry Pi 4 - Model B is a well-known credit card-sized computer. This little marvel of technology from [Raspberry Pi Ltd.](https://www.raspberrypi.com/) acts as the high-level brain of the Duckiedrone, hosting most of the computation power. 

The Duckiedrone `DD24` model uses the Raspberry Pi 4 - Model B, with 4GB of RAM in its standard configuration. [Technical specifications](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/specifications/) are available on the Raspberry Pi website.

The Duckiedrone is compatible with the Raspberry Pi 5 as well. Both variants are flashed with the same `ente` image through `dts init_sd_card` — see [](sw-initialization).

(component-64gb-microsd-card-class-10-u3)=
## 64GB MicroSD card - Class 10 U3

```{figure} ../_images/components-official-dd24/sd-card-64-class10.jpg
:name: sd-card-64
:width: 300px
:align: center
:alt: SD card

64GB Class 10 mini SD card
```

The microSD card is the core memory of the Drone. 

While it does look like a regular SD card with 64GB of nominal capacity, Duckietown microSD cards are "fast", i.e., they have rather high minimum read and write speeds. Fast communication between the drone "brain" (the Raspberry Pi) and memory is needed to prevent lags in the feedback loop. 

```{note}

The microSD card is inserted directly in the Raspberry Pi. Do not use the microSD adapter below during flight.

```

(component-microsd-to-usb-adapter)=
## MicroSD to USB adapter

```{figure} ../_images/components-official-dd24/sd-card-reader.jpg
:name: sd-card-reader
:width: 300px
:align: center
:alt: SD card USB adapter

SD card USB adapter
```
This USB-A microSD adapter is included in the box to support the first initialization of the microSD card. 

[One of the very first steps](sw-initialization) in the assembly process is to insert the microSD card into the adapter, and plug the adapter into your base station (laptop or desktop, not provided in the box) to install the correct software on the Duckiedrone. 

```{warning}
Do not plug the microSD card adapter into the Raspberry Pi during flight. The microSD card goes directly into the Raspberry Pi.
```

(component-rpi-camera-g-fisheye-lens)=
## RPi Camera (G) with Fisheye lens and cable

```{figure} ../_images/components-official-dd24/dd24-camera-and-cable.jpg
:name: rpi-cable-and-cable
:width: 300px
:align: center
:alt: RPI camera and fisheye lens

Raspberry Pi camera with Fisheye lens, and cable
```

The camera is an important sensor for Duckiedrone autonomous operations, allowing it to perceive the environment visually. 

This is an OV5647 5MP [Raspberry Pi Camera (G)](https://waveshare.com/RPi-Camera-G.htm) with a 160-degree field of view, thanks to the included fisheye lens, and (manually) adjustable focus distance. The package includes a 30cm camera cable.

(component-tof-sensor-vl53l1x-black-pcb)=
## Time of Flight Sensor

```{figure} ../_images/components-official-dd24/dd24-tof-back-front.png
:name: tof-sensor-front-back
:width: 300px
:align: center
:alt: Duckietown time of flight sensor v1.4

Time of Flight Sensor (back, front)
```

Time of flight (ToF) sensors are distance measurement sensors. The Duckiedrone mounts five, one on each side and one looking down to measure altitude. The principle of operation is the measurement of the return time of light bouncing off obstacles, so we can think of them as 1D lidars.

The Duckiedrone ToFs mount the VL53L1X module ([VL53L1X specifications](https://learn.adafruit.com/adafruit-vl53l0x-micro-lidar-distance-sensor-breakout)) and come with 15cm, and 23cm 4-pin JST 1.5mm cables. 

(component-motors-cw-lhi-dx2205-2300kv)=
## Motors (CW)

```{figure} ../_images/components-official-dd24/dd24-motors-black-cw.jpg
:name: dd24-motors-cw
:width: 300px
:align: center
:alt: Brushless DC Motors (Clockwise - CW)

Brushless DC Motors (Clockwise - CW) 
```

The Duckiedrone (`DD24`) mounts four brushless DC motors, model DX2205. These racing drone motors feature a motor constant of 2300KV, a M5 shaft diameter, M3 mounting holes and weigh roughly 28g each. 

Note that there are two pairs of motors, distinguishable by the color of the top nut. 

```{note}
**Black** nuts are for motors that spin in the **clockwise (CW)** direction. 
```

If you try to unscrew the top nuts, you will notice that they have opposite threads. This is to prevent the nuts from coming off (along with the propellers) during flight.

(component-motors-ccw-lhi-dx2205-2300kv)=
## Motors (CCW)

```{figure} ../_images/components-official-dd24/dd24-motors-red-ccw.jpg
:name: dd24-motors-ccw
:width: 300px
:align: center
:alt: Brushless DC Motors (Counter-clockwise - CCW)

Brushless DC Motors (Counter-clockwise - CCW) 
```

These motors are the same model as the [](component-motors-cw-lhi-dx2205-2300kv), but are designed for spinning in counter-clockwise direction.

```{note}
**Red** nuts are for motors that spin in the **counter-clockwise (CCW)** direction. 
```

(component-propellers-cw-diatone-polycarbonate-4040)=
## Propellers (CW and CCW) 

```{figure} ../_images/components-official-dd24/props-cw-ccw-arrows.png
:name: dd24-propellers
:width: 400px
:align: center
:alt: Drone propellers

Propellers (CW and CCW) 
```

The Duckiedrone (`DD24`) mounts four Diatone Polycarbonate 4040, 4x4 (in) three-bladed propellers ("props"), each weighing 3.5g. 

The box contains a full spare set, i.e., 4x CW and 4x CCW props. 

As for the motors, it is important to note that one set of these propellers is designed for clockwise (CW) motor operations, while the other for counter-clockwise (CCW). 

To distinguish CW from CCW propellers, find the arrows impressed on the backside, as shown in [](dd24-propellers).


(component-lipo-battery-1500mah-4s-148v-xt60)=
## Battery 

```{figure} ../_images/components-official-dd24/dd24-battery.jpg
:name: dd24-battery
:width: 300px
:align: center
:alt: Duckietown Duckiedrone DD24 LiPo battery

The Duckiedrone battery
```

The battery provides power for the Duckiedrone to operate, and is therefore essential to autonomous operations. 

The Duckiedrone battery is a Lithium-Ion Polymer (LiPo) battery ([LiPo basics on Wikipedia](https://en.wikipedia.org/wiki/Lithium_polymer_battery)) with the following technical specifications:

- Cells: 4
- Output Voltage: 14.8V
- Capacity: 1500mAh
- Discharge Rating: 35C
- Weight: 178g
- Dimensions: 16x35x33mm

Here is good external guide to learn about what each number means: [Rogers Hobby Center LiPo Guide](https://www.rogershobbycenter.com/lipoguide).

```{attention}
Before doing anything with the Duckietown battery, ready the [Duckietown Safety Guidelines](prelim-drone-safety).
```

```{warning}
Lithium-Ion batteries are fire hazards and must be handled with care. 

If for any reason your battery looks punctured or otherwise damaged, do not use it and dispose of it immediately. 

You can learn how to safely dispose of a LiPo battery, e.g., here: [Oscar Liang's Guide on Disposing LiPo batteries](https://oscarliang.com/dispose-lipo-battery-safely/). 
```

The Duckietown battery connects to the drone through a 10cm long XT60 connector, and should be charged using the provided battery charger.

(component-battery-charger-2-4s-liion-lipoly-25w)=
## Battery charger

```{figure} ../_images/components-official-dd24/battery-charger.jpg
:name: dd-battery-charger
:width: 300px
:align: center
:alt: Battery charger 

Lithium Ion battery charger
```

This battery charger provides a safe charging interface for the [LiPo battery](component-lipo-battery-1500mah-4s-148v-xt60). It connects on one side to the battery, and the other to a stable power source (e.g., a wall outlet) through the [battery charge adapter](component-battery-charger-adapter-12v-2a-us-plug).

- Operating Voltage Range: 9V-16V DC
- Cells Type Supported: 2-4 cells Li- Ion/Li-Poly 
- Max Charge Power: 25W
- Charge Current: 1500mA
- Charge Accuracy: ±10mV
- Balance Current: 1000mA
- Material: Metal Case & Voltage Display

(component-battery-charger-adapter-12v-2a-us-plug)=
## Battery charger adapter - 12V 2A US plug

```{figure} ../_images/components-official-dd24/power-adapter.jpg
:name: dd21-dd24-battery-charger-adapter-plug
:width: 300px
:align: center
:alt: Duckiedrone battery charger adapter (12V, 2A)

Duckiedrone battery charger adapter (12V, 2A) 
```

This 12V 2A power adapter, provided with a US wall plug, connects to the battery charger to deliver charge to the battery when needed.

(component-duckietown-drone-hut-v1-2)=
## Duckietown drone HUT - v1.2

```{figure} ../_images/components-official-dd24/dd-hut-v1-2.png
:name: dd24-hut-v-1-2
:width: 300px
:align: center
:alt: Duckiedrone Hut DDHUT v1.2 

The Duckiedrone HUT connects the Duckiedrone embedded systems to the Raspberry Pi, and provides a playground for circuit testing.  
```

The Duckiedrone HUT (yes, HUT, not HAT) is a board that acts as hub for connections between the various peripherals of the drone, such as sensors and flight controller, and the onboard Raspberry Pi. 

It supports the placing of a breadboard to provide a "playground" for additional prototyping or pedagogical experiences, such as adding status LEDs. 

(component-breadboard-yellow-45x35mm)=
## Breadboard - Yellow, 45x35mm

```{figure} ../_images/components-official-dd24/dd24-hut-breadboard.jpg
:name: dd24-breadboard
:width: 300px
:align: center
:alt: Duckiedrone Duckiehut breadboard

Duckiedrone Duckiehut breadboard
```

This is a prototyping breadboard with two-sided tape on the back. Attach it in the appropriate empty region on the top of the [Duckiehut](component-duckietown-drone-hut-v1-2). To learn more about how to use breadboards check, e.g., the [Breadboard Wikipedia](https://en.wikipedia.org/wiki/Breadboard) page.

(component-flight-controller-esc-stack-speedybee-f405-v3)=
## Flight Controller & ESC stack - SpeedyBee F405 V3 BLS 50A

```{figure} ../_images/components-official-dd24/FC-ESC-SpeedyBee.jpg
:name: flight-controller-and-esc-stack-speedybee-box
:width: 300px
:align: center
:alt: The Duckiedrone DD24 flight controller (FC) and electronic speed controllers (ESC) stack 

The flight controller (FC) and electronic speed controllers (ESC) stack 
```
### The flight controller (FC) and Electronic Speed Controller (ESC) - foreword

The **Flight controller (FC)** is the low-level brain of the Duckiedrone, tasked with transforming high-level decisions, e.g., "go faster", into actual commands to the motors. The FC moreover hosts sensors such as the Inertial Measurement Unit (IMU), which measures linear and angular accelerations at high frequency (~200Hz), and a barometer, which indirectly measures height through variations in atmospheric pressure. 

Overall, the FC is an essential component of every drone, even when another computational unit is available onboard (e.g., the Raspberry Pi, as in the case of the Duckiedrone). This is because the dynamics of a drone are much faster than the capability of a Raspberry Pi to deliver commands, e.g., to execute route corrections, especially when the Raspberry Pi is tasked with many other processes as well, as, e.g., visual perception.

The Electronic Speed Controller board, that stacks with the FC and is conveniently included in this same box, transforms speed signals for the motors from the FC into lower-level (PWM) signals that make the motors spin. 

This FC+ESC stack includes the power distribution circuitry as well, receiving power directly from the battery through an XT60 connector and appropriately regulating (adjusting voltage output and stability) it before providing it to various peripherals. 

### FCs on the DD24

```{note}
The model of the FC+ESC stack provided in the box has been upgraded from version F405 V3 50A to F405 V4 55A in April 2025, due to supply chain constraints.
```

The DD24 uses a [SpeedyBee F405 V3 50A](https://www.speedybee.com/speedybee-f405-v3-bls-50a-30x30-fc-esc-stack/), with details provided in [](fig-dd24-fc-esc-405v3-specs).

```{figure} ../_images/components-official-dd24/f405-V3-specification-8.jpg
:name: fig-dd24-fc-esc-405v3-specs
:width: 500px
:align: center
:alt: Duckiedrone DD24 flight controller and esc specifications (speedybee F405v3)

The FC+ESC F405 V3 stack box components and specifications
```

* [Download the F405v3 50A FC&ESC stack technical manual](https://store-fhxxhuiq8q.mybigcommerce.com/product_images/img_SpeedyBee_F405_V3_Stack/SpeedyBee_f405_v3_stack_manual_en.pdf)
* [Download the F405v4 55A FC&ESC stack technical manual](https://store-fhxxhuiq8q.mybigcommerce.com/product_images/img_SpeedyBee_F405_V4_Stack/SpeedyBee_F405_V4_Stack_Manual_EN.pdf)


(component-buzzer-model-2312-3v-24v)=
## Buzzer

```{figure} ../_images/components-official-dd24/dd24-buzzer-f-b.png
:name: fig-dd24-buzzer-front-back
:width: 300px
:align: center
:alt: Duckiedrone buzzer

The buzzer notifies the user when the battery voltage is lower than a certain threshold
```

The Duckiedrone box includes a buzzer. Buzzers emit loud noises when the measured output battery voltage is less than a certain threshold, indicating that the battery is about to be completely discharged. It is a good idea to land the drone when hearing the buzzer buzz. 

(dd24-usb-cables)=
## Cables

```{figure} ../_images/components-official-dd24/dd24-cables.jpg
:name: fig-dd24-cables
:width: 400px
:align: center
:alt: Duckiedrone cables

Duckiedrone cables
```

The Duckiedrone (`DD24`) box includes:

- 1x USB-A to USB-C cable (with data) - 23cm + length of connectors, angled: FC to Raspberry Pi connection 
- 1x USB-C power cable (power only) - 8cm wires + 1cm exposed wire + 2cm connector, exposed wires: FC to Raspberry Pi connection
- 4x 4-pin JST 1.5mm connectors (both ends) - 15cm: ToF sensor to HUT connections
- 1x 4-pin JST 1.5mm connectors (both ends) - 23cm: ToF sensor to HUT connection

(component-heat-sink-kit-multiple-sizes)=
## Heat sink kit

```{figure} ../_images/components-official-dd24/heat-sinks.jpg
:name: fig-rpi-heat-sinks
:width: 300px
:align: center
:alt: Heat sinks for DD24 Raspberry Pi

Heat sinks mount on the Raspberry Pi and help dissipate heat and lower the temperature of the board
```

Heat sinks are simple passive convective elements that stick to specific locations on the Raspberry Pi. Heat sinks help reduce the temperature of the Raspberry Pi preventing it from automatically going in protection mode and partially shutting down computational resources.

(component-40mm-gpio-fan-pwm-speed-control-3pin)=
## Fan

```{figure} ../_images/components-official-dd24/dd24-fan-pwm.png
:name: fig-fan-40x40
:width: 300px
:align: center
:alt: Duckiedrone PWM fan for Raspberry Pi temperature control

PWM fan for Raspberry Pi temperature control
```

This fan is used to cool down the Raspberry Pi. It is a PWM fan, meaning the speed at which it spins can be controlled.

 - PWM speed control, 3-pin Dupont connectors
 - Size: 40x40x10mm 
 - 85mm long cables

<!--
(component-40mm-gpio-fan-reference-model)=
## 40mm GPIO Fan - Reference model
-->

(dd24-header-jumper)=
## Header jumpers - 2.54mm spacing, black

```{figure} ../_images/components-official-dd24/dd24-jumper.png
:name: fig-jumper-2-54mm
:width: 300px
:align: center
:alt: Jumper for Wi-Fi mode toggling

Jumper for Wi-Fi mode manual toggling
```

This 2.54mm jumper is used to short (i.e., connect) pins on the HUT. This is useful, e.g., to activate different Wi-Fi modes. 


(component-velcro-strips-10inch-black)=
## Velcro strips

```{figure} ../_images/components-official-dd24/velcro.jpg
:name: dd24-velcro
:width: 300px
:align: center
:alt: Duckiedrone velcro strips

Velcro strips
```

Velcro strips are used for cable management and securing components, such as the battery, to the drone's chassis. The Duckiedrone box contains:

- 2x 25cm black Velcro strips

(dd24-chassis)=
## Chassis

The Duckiedrone's chassis is a custom designed 2.5mm thick carbon fiber sandwich-design chassis, with the following components.  

(component-bottom-plate-carbon-fiber-25mm)=
### Bottom Plate
```{figure} ../_images/components-official-dd24/chassis-bottom.jpg
:name: fig-dd24-chassis-bottom
:width: 300px
:align: center
:alt: Duckiedrone chassis bottom plate

Duckiedrone chassis: bottom plate  
```

The bottom part of the chassis provides the main surface for mounting components, such as the motors, the electronics, the battery and more. 

The chassis has a square design with each side measuring 27.2cm.

(component-top-plate-carbon-fiber-25mm)=
### Top Plate

```{figure} ../_images/components-official-dd24/chassis-top.jpg
:name: fig-dd24-chassis-top
:width: 300px
:align: center
:alt: Duckiedrone chassis bottom plate

Duckiedrone chassis: top plate   
```

The top chassis plate has the same outer dimensions as the bottom plate (27.2cm side), and its main function is to provide structural stability to the drone.

(component-tof-support-carbon-fiber-25mm)=
### Other chassis components 

```{figure} ../_images/components-official-dd24/chassis-support.jpg
:name: fig-dd24-chassis-support
:width: 300px
:align: center
:alt: Duckiedrone chassis other components

Duckiedrone chassis: other components 
```
Other chassis components include:

- 2x ToF sensor vertical supports: for mounting side viewing ToF sensors
- 2x Fan duct walls: to improve thermal control efficiency of the fan
- 1x Battery support plate: to hold the battery.

(component-camera-tof-mount-abs-material)=
### Camera mount with ToF sensor support

```{figure} ../_images/components-official-dd24/dd24-camera-mount.jpg
:name: fig-dd24-camera-mount
:width: 300px
:align: center
:alt: Duckiedrone camera mount

Camera mount with integrated time of flight sensor mounting support 
```

This chassis component mounts on the bottom plate and is used to hold the Duckiedrone's camera and front-facing ToF sensor. It places the camera at a 60-degree angle. Coupled with the camera's 160-degree field of view, it allows the drone to see both underneath and in front at the same time. 


(component-landing-gear-3d-printed-v11)=
### Landing Gear - 3D printed v1.1

```{figure} ../_images/components-official-dd24/dd24-landing-gears.jpg
:name: fig-dd24-landing-gear
:width: 300px
:align: center
:alt: Duckiedrone landing gear

Landing gear 
```

Landing gears mount under each motor and support the Duckiedrone when landing. 

(component-rubber-pads)=
## Landing Pads

```{figure} ../_images/components-official-dd24/landing-pads.jpg
:name: fig-dd24-landing-pads
:width: 300px
:align: center
:alt: Duckiedrone rubber landing pads

Rubber landing pads
```

These rubber landing pads mount under the landing gears and provide a softer landing experience.

(component-dd24-bits-and-pieces)=
## Bits and Pieces (Screws, nuts, standoffs)

```{figure} ../_images/components-official-dd24/dd24-bits.jpg
:name: fig-dd24-screws-nuts-standoffs
:width: 300px
:align: center
:alt: Duckiedrone screws, nuts, standoffs

Screws, nuts and standoffs
```

The Duckiedrone box includes a host of "bits and pieces", including spares of each:

- **Screws** (number + spares)
  - (22+2)x Nylon (M3x6) - 16x chassis, 2x securing battery, 4x forward battery supports
  - (3+1)x Nylon (M3x10) - 3x camera mount
  - (16+2)x Metal (M3x14) - 16x motors + landing gears
  - (4+1)x Metal (M3x20) - 4x FC
  - (11+2)x Nylon (M2.5x8) - 4x HUT, 5x ToF sensors, 2x aft battery supports, 2x Raspberry Pi stack 
  - (6+1)x Nylon (M2x10) - 4x camera, 2x buzzer 
- **Nuts**
  - (5+1)x Nylon (M2.5) - 5x ToF sensors
  - (6+1)x Nylon (M2) - 4x camera, 2x buzzer
  - (5+1)x Nylon (M3) - 2x securing battery, 3x camera mount
- **Standoffs**
  - (2+1)x Nylon (M2.5x40+6 MF) - 2x battery plate and Raspberry Pi standoffs
  - (4+1)x Nylon (M2.5x20+6 MF) - 4x Raspberry Pi and bottom plate
  - (4+1)x Nylon (M2.5x15+6 FF) - 4x Raspberry Pi and HUT
  - (8+2)x Nylon (M3x35+6 FF) - 8x chassis
  - (2+1)x Nylon (M3x40+6 FF) - 2x forward battery supports
  - (2+1)x Nylon (M3x6+6 MF) - 2x fan


(component-8mm-wrench-motor-check)=
## Wrench 

```{figure} ../_images/components-official-dd24/wrench.jpg
:name: fig-dd24-wrench
:width: 300px
:align: center
:alt: 8mm wrench

Wrench (8mm) for tightening motor top nuts 
```

This 8mm wrench is provided to tighten up the motor-prop nuts. We really do not want those to go off while flying.

(component-screwdriver-cross)=
## Screwdriver - Cross

```{figure} ../_images/components-official-dd24/screw-driver.jpg
:name: fig-dd24-screw-driver
:width: 300px
:align: center
:alt: Philips screwdriver

Cross screwdriver  
```
This simple Philips screwdriver drives all the screws in the box.  

(component-camera-calibration-pattern)=
## Camera Calibration pattern

```{figure} ../_images/components-official-dd24/dd24-camera-calibration-pattern.jpg
:name: dd24-camera-calibration-pattern
:width: 300px
:align: center
:alt: Duckiedrone and Duckiebot camera calibration pattern

Duckiedrone (and Duckiebot) camera calibration pattern 
```

This checkerboard pattern is used to calibrate the Duckiedrone camera.


(component-rubber-duckies-duckietown-swag)=
## Rubber Duckies

```{figure} ../_images/components-official-dd24/duckies.jpg
:name: fig-duckietown-duckies
:width: 300px
:align: center
:alt: Duckietown Duckies

Duckietown duckies
```

Duckietown duckies are non-functional yet essential to the operations of the Duckiedrone. Make sure to always have at least one duckie onboard. 

(component-duckietown-stickers-duckietown-swag)=
## Duckietown Stickers

```{figure} ../_images/components-official-dd24/dt-stickers.jpg
:name: fig-duckietown-stickers
:width: 300px
:align: center
:alt: Duckietown stickers

Duckietown stickers
```

Duckietown stickers look great on your laptop, and notify others of your Duckietown training.    

(component-instruction-card-drone-assembly)=
## Duckiedrone instruction card

```{figure} ../_images/components-official-dd24/dd24-instructions.jpg
:name: fig-dd24-instructions
:width: 300px
:align: center
:alt: Duckiedrone DD24 instructions card

Duckiedrone DD24 instruction card 
```

This instruction card provides links to the [Duckietown website](https://duckietown.com) and the [Duckietown get started page](https://duckietown.com/get-started/).