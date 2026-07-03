```{seo}
:description: Flash the Bluejay firmware onto the four ESCs of the Duckietown Duckiedrone model DD24 using the esc-configurator.com web tool.
:keywords: Duckiedrone, Duckietown, autonomous drone, uav, ESC, Bluejay, BLHeli_S, esc-configurator, mamba-f405-mk2
```

```{needget}
* A base station computer with a Chrome-based browser
* Flight Controller
* USB to USB-C cable
* A charged LiPo battery
---
* Four ESCs running the Bluejay firmware
```

(dd24-esc-init)=
# Initializing the ESCs

The four Electronic Speed Controllers (ESCs) translate the Flight Controller's commands into the currents that drive the motors. In this section you will flash the [Bluejay](https://github.com/bird-sanctuary/bluejay) firmware onto the four ESCs using the [esc-configurator.com](https://esc-configurator.com/) web tool.

```{danger}
**Remove the propellers** and keep them off for the entire procedure.
```

## 1. Flash Betaflight onto the Flight Controller

The ESC tool does not talk to the ESCs directly. It reaches them by passing through the Flight Controller, and for that to work the Flight Controller needs to be running **Betaflight** (target firmware `BTFL 4.3.2`). The Duckiedrone's Flight Controller does not come with Betaflight, so the first thing to do is flash it.

Start by installing [Betaflight Configurator **v10.9.0**](https://github.com/betaflight/betaflight-configurator/releases/tag/10.9.0) on your base station and opening it.

Next, open the `Firmware Flasher` tab from the left sidebar and set the options to match your board, as shown below. The Duckiedrone uses the Mamba F405 MK2, so select the `MAMBAF405_2022B` target and the `BTFL 4.3.2` firmware version.

```{figure} ../_images/esc-init/flasher_parameters_mamba.png
:width: 500px

`Firmware Flasher` settings for the Mamba F405 MK2 (`BTFL 4.3.2`)
```

### Putting the Flight Controller into bootloader mode

Before you can write new firmware, the Flight Controller has to be in **bootloader mode** (also called DFU mode). When you plug the board in normally it boots straight into its regular firmware and will not accept a flash, so you need to hold it in bootloader mode while it powers up.

You put the board into bootloader mode with the small `BOOT` button on the Flight Controller. Locate it first:

```{figure} ../_images/esc-init/mamba_boot_button.png
:width: 500px

`BOOT` button location on the Mamba F405 MK2 Flight Controller.
```

With the board still disconnected, press and hold the `BOOT` button, connect the USB-C cable to your base station while keeping it held, and then release the button. The board is now waiting in bootloader mode.

```{attention}
Do not press the `"Connect"` button in Betaflight Configurator during flashing. Betaflight talks to the board over the flashing interface, not the normal serial connection.
```

### Loading and flashing the firmware

Now that the board is in bootloader mode, download the firmware and write it:

1. Click `Load Firmware [Cloud]` in the bottom-right corner. Betaflight Configurator fetches the `BTFL 4.3.2` firmware for the target you selected and confirms it is ready to flash.

1. Click `Flash Firmware`, also in the bottom-right corner, and watch the progress bar as it works through the write.

   ```{admonition} Check
   :class: seealso

   The progress bar moves through `"Flashing…"`, then `"Verifying…"`, and finally `"Programming SUCCESSFUL"`.
   ```

Once the flash finishes, the board reboots on its own into the new firmware, so there is no need to unplug it. You can confirm it worked by clicking `"Connect"`: the top-left of the interface should report `BTFL 4.3.2`. When you have checked the version, click `"Disconnect"` so the serial port is free for the ESC tool in the next step.

## 2. Connect to the ESCs

```{note}
Use a Chrome-based browser (Google Chrome, Microsoft Edge, and so on).
```

```{attention}
The ESCs cannot draw power from the Flight Controller over USB. You have to connect the LiPo battery for them to power up and accept programming.
```

1. Connect the LiPo battery to the drone.

1. In Chrome, go to the [ESC Configurator](https://esc-configurator.com/) web tool.

1. Click `Select Serial Port` in the top-right corner. In the prompt that appears, pick the same port the Flight Controller is on and click `Connect`.

   ```{figure} ../_images/esc-init/esc-config-serial-port-selection.png

   Serial port selection prompt.
   ```

## 3. Flash Bluejay

1. Click `Read Setup`. The tool passes through the Flight Controller and reads all four ESCs. Each one comes up as a `BLHeli_S` ESC on layout `J-H-15`.

1. In the firmware selection, set:
   * **Firmware:** `Bluejay`
   * **Version:** `0.21.0`
   * **PWM frequency:** `48 kHz`

   ```{todo}
   Add a screenshot of the Bluejay firmware-selection dialog in esc-configurator.com.
   ```

1. Click `Flash All` and wait for all four ESCs to finish.

   ```{attention}
   Do not disconnect the battery or the USB cable while an ESC is being flashed.
   ```

1. When the flashing completes, click `Read Setup` once more and check that all four ESCs now report `Bluejay 0.21.0`.

## 4. Disable the beacon

By default the ESCs give off a beep after a while without commands, which quickly gets annoying. While you are still in the ESC Configurator, turn it off:

1. In the `Beacon Delay` drop-down menu, select `Infinite`.

   ```{figure} ../_images/esc-init/esc-config-disable-beacon.png

   Set `Beacon Delay` to `Infinite` to disable the beacon sound.
   ```

1. Click `Write Settings` in the bottom-right corner to save.

Your ESCs are now running Bluejay. Next, continue to [Initializing the Flight Controller](dd24-fc-init) to flash the PX4 firmware.

(esc_init_troubleshooting)=
## Troubleshooting

```{trouble}
The ESC Configurator does not detect any ESCs after `Read Setup`.
---
The ESCs are not powered. Check that the LiPo battery is connected to the drone, since the Flight Controller's USB power is not enough to run the ESCs, and then click `Read Setup` again.
```

```{trouble}
On Linux, the ESC Configurator cannot open the serial port (`Failed to open serial port`).
---
This is a serial-port permission issue. Add your user to the `dialout` group by running `sudo usermod -a -G dialout <username>` (replacing `<username>` with your base station username), then reboot for the change to take effect. If that does not help, run `sudo chmod 0777 /dev/ttyACM0` (substituting the port you are using) while the tool is open; this has to be repeated each time the Flight Controller is reconnected.
```

```{trouble}
Other issues.
---
We are happy to support! Please contact our hardware team via email: [hardware@duckietown.com](mailto:hardware@duckietown.com)
```
