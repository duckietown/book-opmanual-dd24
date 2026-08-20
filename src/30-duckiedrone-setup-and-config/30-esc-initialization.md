```{seo}
:description: Flash the Bluejay firmware onto the four ESCs of the Duckietown Duckiedrone DD24-B using the esc-configurator.com web tool.
:keywords: Duckiedrone, Duckietown, autonomous drone, uav, ESC, Bluejay, BLHeli_S, esc-configurator, mamba-f405-mk2
```

```{needget}
* A base station computer with a Chrome-based browser
* Flight Controller
* USB-A to USB-C cable
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

## 1. (skip if not necessary) Flash Betaflight onto the Flight Controller (FC)

 ```{attention}
This step should not be necessary as the `DD24-B` ships with Betaflight 4.3.2 BTFL, Target DIAT/FURYF4OSD(STM32F405) already installed on the FC. Follow this step only if this is not the case.
```

The ESC configuration tool does not talk to the ESCs directly, but reaches them through the FC. For this to work, **Betaflight** (target firmware `BTFL 4.3.2`) needs to be flashed on the FC. If the Duckiedrone's FC does not already have Betaflight installed, go ahead and flash it.

Start by installing [Betaflight Configurator **v10.9.0**](https://github.com/betaflight/betaflight-configurator/releases/tag/10.9.0) on your base station and opening it.

Next, open the `Firmware Flasher` tab from the left sidebar and set the options to match your board, as shown below. The Duckiedrone uses the Mamba F405 MK2, so select the `MAMBAF405_2022B` target and the `BTFL 4.3.2` firmware version.

```{figure} ../_images/esc-init/flasher_parameters_mamba.png
:width: 500px

`Firmware Flasher` settings for the Mamba F405 MK2 (`BTFL 4.3.2`)
```

### Connecting the Flight Controller and loading it into bootloader mode

Before you can write new firmware, the Flight Controller has to be in **bootloader mode** (also called DFU mode). When you plug the board in normally it boots straight into its regular firmware and will not accept a flash, so you need to hold it in bootloader mode while it powers up.

You put the board into bootloader mode with the small `BOOT` button on the Flight Controller. Locate it first:

```{figure} ../_images/esc-init/mamba_boot_button.png
:width: 500px

`BOOT` button location on the Mamba F405 MK2 Flight Controller.
```

 ```{todo}
Clarify the connection and initial conditions. Should the RPI be on or off? Can I use the same cable that connects the RPI to the FC, unplugging it temporarily from the Pi, or do I need another cable? 
   ```

With the board still disconnected, press and hold the `BOOT` button while you connect the USB-C cable to your base station, and then release the button. The board is now waiting in bootloader mode.

 ```{admonition} Check
:class: seealso

With the Flight Controller connected in bootloader mode, the port/target dropdown in the top toolbar of Betaflight Configurator reads `DFU - STM32 BOOTLOADER`, and the `Connect` button is greyed out, since the board is not reachable over the normal serial link while in DFU mode.
   ```

```{figure} ../_images/esc-init/betaflight_dfu_bootloader_dropdown.png
:width: 500px

The Flight Controller correctly in bootloader mode, as shown by `DFU - STM32 BOOTLOADER` in Betaflight Configurator.
```

If the dropdown does not show `DFU - STM32 BOOTLOADER`, see [](esc_init_troubleshooting).

```{attention}
Do not press the `"Connect"` button in Betaflight Configurator during flashing. Betaflight talks to the board over the flashing interface, not the normal serial connection.
```

### Loading and flashing the firmware

Now that the board is in bootloader mode, download the firmware and write it:

1. Click `Load Firmware [Online]` in the bottom-right corner. Betaflight Configurator fetches the `BTFL 4.3.2` firmware for the target you selected and confirms it is ready to flash.

1. Click `Flash Firmware`, also in the bottom-right corner, and watch the progress bar as it works through the write.

   ```{admonition} Check
   :class: seealso

   The progress bar moves through `"Flashing…"`, then `"Verifying…"`, and finally `"Programming SUCCESSFUL"`.
   ```

Once the flash finishes, the board reboots on its own into the new firmware, so there is no need to unplug it. You can confirm it worked by clicking `"Connect"`: the top-left of the interface should report `BTFL 4.3.2`.

### Loading the Duckiedrone board configuration

A freshly flashed board comes up with the stock Betaflight defaults, which do not match the Duckiedrone's wiring. In particular, the motor outputs and the serial port assignments have to be remapped before the ESC tool can reach the four ESCs. Duckietown provides these settings as a Betaflight CLI configuration file that is applied in one paste.

1. With the Flight Controller connected in Betaflight Configurator (click `"Connect"` if it is not connected already), open the `CLI` tab from the left sidebar.

1. Open [`MAMBAF405MK2V2.conf`](https://raw.githubusercontent.com/duckietown/pidrone_pkg/b859bf21a5b2e0457daecfe08896a9cd2a6de88f/MAMBAF405MK2V2.conf) and copy its entire contents.

1. Paste the contents into the CLI command box and press `Enter`. The commands run as a batch, and the file ends with `save`, so the board writes the settings and reboots on its own.

   ```{attention}
   Do not disconnect the USB cable while the batch is running.
   ```

   ```{admonition} Check
   :class: seealso

   The CLI output ends without reporting any command errors, and Betaflight Configurator drops the connection as the board reboots.
   ```

When the configuration is in place, click `"Disconnect"` so the serial port is free for the ESC tool in the next step.

## 2. Connect to the ESCs

```{note}
Use a Chrome-based browser (Google Chrome, Microsoft Edge, and so on).
```

1. Make sure the motors **do not** have propellers attached.

1. Check that the ESC board is wired to the Flight Controller. The ESC tool reaches the ESCs only through this connection, so the ribbon connector between the ESC board and the Flight Controller has to be fully seated at both ends, with all four motor leads attached to the ESC board.

1. Disconnect Betaflight (if on), or any other programs that might be using the serial port the flight controller is connected to.

1. In Chrome, go to the [ESC Configurator](https://esc-configurator.com/) web tool.

1. Click `Select Serial Port` in the top-right corner. In the prompt that appears, select the serial port whose name contains `Betaflight STM32F405`. If multiple USB serial devices are connected, unplug the Flight Controller and observe which port disappears, then reconnect it and select the port that reappears.

1. Once the port is identified, select it and click `Connect`.

   ```{figure} ../_images/esc-init/esc-config-serial-port-selection.png

   Serial port selection prompt.
   ```

1. Verify the message "Unique device ID received" appears (this means your FC can talk to the ESC).

   ```{figure} ../_images/esc-init/esc-config-unique-id.png
   :width: 80%
   :name: esc-config-unique-id
   :alt: verifying the unique device ID received message when first connecting to the duckiedrone's ESCs

   Verify a Unique device ID has been received.
   ```

```{attention}
The ESCs cannot draw power from the Flight Controller over USB. You have to connect the LiPo battery for them to power up and accept programming. If this is the first time you are plugging in the battery, be particularly careful to notice if anything is becoming particularly hot, and be ready to unplug the battery if so.
```

1. Connect the LiPo battery to the Duckiedrone. Bluejay will detect it:

   ```{figure} ../_images/esc-init/01-esc-configurator-battery-plugin.jpg
   :width: 80%
   :name: 01-esc-configurator-battery-plugin
   :alt: Verifying the battery connection for a Duckiedrone with Bluejay

   Bluejay will detect the battery when plugged in.
   ```

## 3. Flash Bluejay

1. Click `Read Setup`. The tool passes through the Flight Controller and reads all four ESCs. Each one comes up as a `BLHeli_S` ESC on layout `J-H-15`.

   ```{figure} ../_images/esc-init/02-esc-configurator-reading-escs.jpg
   :width: 80%
   :name: 02-esc-configurator-reading-escs
   :alt: Duckiedrone ESC default factory configuration as read by Bluejay

   The Duckiedrone DD24-B ESCs ship with BLHeli S on JH15 layout. 
   ```

1. In the firmware selection, set:
   * **Firmware:** `Bluejay`
   * **Version:** `0.21.0`
   * **PWM frequency:** `48 kHz`

   ```{figure} ../_images/esc-init/03-esc-configurator-config-flashing-escs.jpg
   :width: 80%
   :name: 03-esc-configurator-config-flashing-escs
   :alt: Flashing the Duckiedrone ESCs with Bluejay

   Upgrading the ESC firmware with specific Bluejay parameters.
   ```

1. Click `Flash All` and wait for all four ESCs to finish.

   ```{attention}
   Do not disconnect the battery or the USB cable while an ESC is being flashed.
   ```


1. When the flashing completes, click `Read Setup` once more and check that all four ESCs now report `Bluejay 0.21.0`.

   ```{figure} ../_images/esc-init/04-esc-configurator-after-flashing.jpg
   :width: 80%
   :name: 04-esc-configurator-after-flashing
   :alt: Confirming Bluejay on the Duckiedrone DD24-B ESCs

   Validate that the correct firmware has been successfully flashed. 
   ```


## (optional) 4. Disable the beacon

By default the ESCs give off a beep after a while without commands, which quickly gets annoying. While you are still in the ESC Configurator, turn it off:

1. In the `Beacon Delay` drop-down menu, select `Infinite`.

   ```{figure} ../_images/esc-init/esc-config-disable-beacon.png

   Set `Beacon Delay` to `Infinite` to disable the beacon sound.
   ```

1. Click `Write Settings` in the bottom-right corner to save.

Disconnect the LiPo battery from the Duckiedrone.

Your ESCs are now running Bluejay. Next, continue to [Initializing the Flight Controller](dd24-fc-init) to flash the PX4 firmware.

(esc_init_troubleshooting)=
## Troubleshooting

```{trouble}
The ESC Configurator does not detect any ESCs after `Read Setup`.
---
The ESCs are not powered. Check that the LiPo battery is connected to the Duckiedrone, since the Flight Controller's USB power is not enough to run the ESCs, and then click `Read Setup` again.
```

```{trouble}
On Linux, the ESC Configurator cannot open the serial port (`Failed to open serial port`).
---
This is a serial-port permission issue. Add your user to the `dialout` group by running `sudo usermod -a -G dialout <username>` (replacing `<username>` with your base station username), then reboot for the change to take effect. If that does not help, run `sudo chmod 0777 /dev/ttyACM0` (substituting the port you are using) while the tool is open; this has to be repeated each time the Flight Controller is reconnected.
```

```{trouble}
The port/target dropdown in Betaflight Configurator does not show `DFU - STM32 BOOTLOADER`.
---
The board booted into its regular firmware instead of the bootloader. Unplug the USB-C cable, press and hold the `BOOT` button again, reconnect the cable while still holding the button, and only release it once the cable is fully seated. If it still does not show up, try a different USB cable or port, since some cables are power-only and cannot carry data.
```

```{trouble}
Other issues.
---
We are happy to support! Please contact our hardware team via email: [hardware@duckietown.com](mailto:hardware@duckietown.com)
```
