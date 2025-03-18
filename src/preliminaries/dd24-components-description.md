(prelim-dd24-box-component-description)=
# Description of Components (`DD24`)

Most components in the Duckiedrone box are functional, i.e., the serve a purpose in learning how to or directly flying the drone. Other components are not functional, but still useful. 

We provide below a brief description of each component in the box.

(component-raspberry-pi-4-4gb)=
## Raspberry Pi 4 - Model B - 4GB

```{figure} ../_images/components-official-dd24/rpi4.jpg
:name: rpi4
:width: 300px
:align: center
:alt: Raspberry Pi 4 Model B, 4GB RAM

Raspberry Pi 4 Model B, 4GB RAM
```

The Raspberry Pi 4 - Model B is a well-known credit card-size computer. This little marvel of technology from [Raspberry Pi Ltd.](https://www.raspberrypi.com/) acts as main brain of our Duckiedrone, hosting most of the computation power. 

The Duckiedrone `DD24` model uses the Raspberry Pi 4 - Model B, with 4GB of RAM in its standard configuration. [Technical specifications](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/specifications/) are available on the Raspberry Pi website.

The Duckiedrone is compatible with the Raspberry Pi 5 too, but initialization instructions are different from the 4 version.

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

[One of the very first steps](sw-initialization) in the assembly process is to insert the microSD card in the adapter, and plug the adapter in your base station (laptop or desktop, not provided in the box) to install the correct software on Duckiedrone. 

```{warning}
Do not plug the microSD card adapter in the Raspberry Pi during flight. The microSD card goes directly into the Raspberry Pi.
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

The camera is an extremely important sensor for the Duckiedrone autonomous operation, allowing it to perceive the environment visually. 

This is a OV5647 5MP [Raspberry Pi Camera (G)](https://waveshare.com/product/rpi-camera.htm) with 160 degrees field of view, thanks to the included fisheye lens, and (manually) adjustable focus distance. The package includes a 30cm camera cable.

(component-tof-sensor-vl53l1x-black-pcb)=
## Time of flight sensors sensor

```{figure} ../_images/components-official-dd24/dd24-tof-back-front.png
:name: tof-sensor-front-back
:width: 300px
:align: center
:alt: Duckietown time of flight sensor v1.4

Time of Flight Sensor (back, front)
```

VL53L1X, Black PCB, 5x

ToF sensor cable - 15cm, 4-pin JST 1.5mm
ToF sensor cable - 23cm, 4-pin JST 1.5mm

(component-motors-cw-lhi-dx2205-2300kv)=
## Motors (CW)

```{figure} ../_images/components-official-dd24/dd24-motors-1.jpg
:name: dd24-motors-cw
:width: 300px
:align: center
:alt: Brushless DC Motors (Clockwise - CW)

Brushless DC Motors (Clockwise - CW) 
```

 - LHI DX2205 2300KV

(component-motors-ccw-lhi-dx2205-2300kv)=
## Motors (CCW)

```{figure} ../_images/components-official-dd24/dd24-motors-2.jpg
:name: dd24-motors-cw
:width: 300px
:align: center
:alt: Brushless DC Motors (Clockwise - CCW)

Brushless DC Motors (Counter-clockwise - CCW) 
```

 - LHI DX2205 2300KV

(component-propellers-cw-diatone-polycarbonate-4040)=
## Propellers (CW and CCW) 

```{figure} ../_images/components-official-dd24/dd24-props.jpg
:name: dd24-propellers
:width: 300px
:align: center
:alt: Drone propellers

Propellers (CW and CCW) 
```

- Diatone Polycarbonate 4040

(component-lipo-battery-1500mah-4s-148v-xt60)=
## LiPo Battery - 1500mAh, 4S, 14.8V, XT60 connector

```{figure} ../_images/components-official-dd24/dd24-battery.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-battery-charger-2-4s-liion-lipoly-25w)=
## Battery charger

```{figure} ../_images/components-official-dd24/battery-charger.jpg
:name: dd-battery-charger
:width: 300px
:align: center
:alt: Battery charger 

Lithium Ion battery charger
```

- 2-4S Li-Ion/Li-Poly, 25W

(component-battery-charger-adapter-12v-2a-us-plug)=
## Battery charger adapter - 12V 2A US plug

```{figure} ../_images/components-official-dd24/power-adapter.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-duckietown-drone-hut-v11)=
## Duckietown drone HUT - v1.2

```{figure} ../_images/components-official-dd24/dd-hut-v1-2.png
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-breadboard-yellow-45x35mm)=
## Breadboard - Yellow, 45x35mm

```{figure} ../_images/components-official-dd24/dd24-hut-breadboard.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-flight-controller-esc-stack-speedybee-f405-v3)=
## Flight Controller & ESC stack - SpeedyBee F405 V3

```{figure} ../_images/components-official-dd24/FC-ESC-SpeedyBee.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-buzzer-model-2312-3v-24v)=
## Buzzer - Model 2312, 3V-24V

```{figure} ../_images/components-official-dd24/
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

USB-A to USB-C cable - 23cm, angled
USB-C power cable - 10cm, exposed wires
Header jumpers - 2.54mm spacing, black

(component-heat-sink-kit-multiple-sizes)=
## Heat sink kit - Multiple sizes

```{figure} ../_images/components-official-dd24/heat-sinks.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-40mm-gpio-fan-pwm-speed-control-3pin)=
## 40mm GPIO Fan - PWM speed control, 3-pin

(component-40mm-gpio-fan-reference-model)=
## 40mm GPIO Fan - Reference model

(component-zipties-12cm-black)=
## Zipties - 12cm, black

```{figure} ../_images/components-official-dd24/
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-zipties-25cm-black)=
## Zipties - 25cm, black

```{figure} ../_images/components-official-dd24/
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-velcro-strips-10inch-black)=
## Velcro strips - 10inch, black

```{figure} ../_images/components-official-dd24/velcro.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-bottom-plate-carbon-fiber-25mm)=
## Bottom Plate - Carbon fiber, 2.5mm thick

```{figure} ../_images/components-official-dd24/chassis-bottom.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-top-plate-carbon-fiber-25mm)=
## Top Plate - Carbon fiber, 2.5mm thick

```{figure} ../_images/components-official-dd24/chassis-top.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-tof-support-carbon-fiber-25mm)=
## ToF support - Carbon fiber, 2.5mm thick

```{figure} ../_images/components-official-dd24/chassis-support.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

Fan duct wall - Carbon fiber, 2.5mm thick
Battery support - Carbon fiber, 2.5mm thick

(component-camera-tof-mount-abs-material)=
## Camera + ToF mount - ABS material

```{figure} ../_images/components-official-dd24/dd24-camera-mount.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-landing-gear-3d-printed-v11)=
## Landing Gear - 3D printed v1.1

```{figure} ../_images/components-official-dd24/dd24-landing-gears.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-rubber-pads)=
## Rubber Pads

```{figure} ../_images/components-official-dd24/landing-pads.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-dd24-bits-and-pieces)=
## Bits and Pieces (Screws, nuts, standoffs)

```{figure} ../_images/components-official-dd24/dd24-bits.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

Metal screws (M3x14) - Steel, Phillips head
Nylon screws (M2.5x8)
Nylon Standoff (M2.5x40+6 MF)
Nylon Standoff (M2.5x20+6 MF)
Nylon Standoff (M2.5x15+6 FF)
Nylon Nuts (M2.5) - ToF sensors
Nylon screws (M3x10) - Camera mount
Nylon screws (M2x10) - Camera, Buzzer
Nylon Nuts (M2) - Camera, Buzzer
Nylon screws (M3x6) - Chassis, battery supports
Nylon Standoff (M3x35+6 FF) - Chassis
Nylon Standoff (M3x40+6 FF) - Battery supports
Nylon Standoff (M3x6+6 MF) - Fan mounting
Nylon Nuts (M3) - Battery, camera mount
Metal screws (M3x20) - Flight Controller

(component-8mm-wrench-motor-check)=
## 8mm wrench - Motor compatibility check

```{figure} ../_images/components-official-dd24/wrench.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-screwdriver-cross)=
## Screwdriver - Cross

```{figure} ../_images/components-official-dd24/screw-driver.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-camera-calibration-pattern)=
## Camera Calibration pattern

```{figure} ../_images/components-official-dd24/dd24-camera-calibration-pattern.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-rubber-duckies-duckietown-swag)=
## Rubber Duckies - Duckietown swag

```{figure} ../_images/components-official-dd24/duckies.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-duckietown-stickers-duckietown-swag)=
## Duckietown Stickers - Duckietown swag

```{figure} ../_images/components-official-dd24/dt-stickers.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-instruction-card-drone-assembly)=
## Instruction card - Drone assembly

```{figure} ../_images/components-official-dd24/dd24-instructions.jpg
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-internal-packaging-skin)=
## Internal Packaging Skin

```{figure} ../_images/components-official-dd24/
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-duckiedrone-box)=
## Duckiedrone box

```{figure} ../_images/components-official-dd24/
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```

(component-shipping-package-box)=
## Shipping Package box

```{figure} ../_images/components-official-dd24/
:name: name-unique
:width: 300px
:align: center
:alt: text

Text 
```