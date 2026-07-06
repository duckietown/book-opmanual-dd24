```{seo}
:description: Instructions to initialize the flight controller on the Duckietown Duckiedrone model DD24 by flashing the PX4 bootloader and firmware.
:keywords: Duckiedrone, Duckietown, autonomous drone, uav, flight controller, initialization, PX4, dfu-util, mamba-f405-mk2
```

```{needget}
* A base station computer running Linux (Ubuntu) or macOS
* "Mamba" (`DD24-B`) Flight Controller
* USB to USB-C cable with data
---
* An up-to-date, initialized "Mamba" FC running PX4
```

(dd24-fc-init)=
# Initializing the Flight Controller

The Flight Controller (FC) implements several low-level behaviors, e.g., stabilizing the Duckiedrone around roll, pitch, and yaw through three different PID controllers. Correctly configuring the FC is critical for achieving (safe) flight.

The `DD24-B` runs the [PX4 Autopilot](https://px4.io/) firmware, built for the `mamba-f405-mk2` target. PX4 is a popular, open-source flight control software ultimately directed by the[Linux Foundation](https://www.linuxfoundation.org/). 

To make the flashing process as deterministic as possible, it is performed entirely from the command line with [`dfu-util`](https://dfu-util.sourceforge.net/). In this way sthe procedure is fully scriptable and does not depend on internal mechanics of a graphical flasher.

```{note}
The flashing is a two-stage process:

1. **Flash the PX4 bootloader** (the [`omnibusf4sd_bl`](https://github.com/PX4/PX4-Bootloader) bootloader, which is the canonical bootloader for STM32F405-based boards using PX4 board ID `42`, including the Diatone Mamba F405 MK2).
2. **Flash the PX4 firmware** built for the `mamba-f405-mk2` target on top of the bootloader.

Both stages are performed via `dfu-util` while the FC is in STM32 DFU mode.
```

## 1. Install `dfu-util`

Install `dfu-util` on your base station. Pick the tab matching your operating system.

:::::{tab-set}

::::{tab-item} Linux (Ubuntu)

```bash
sudo apt update
sudo apt install dfu-util
```

```{todo [DTSW-8044]}
"Failed to send reload request: No such file or directory" - uncomment after error solved
```

<!--
(Optional but recommended) install a udev rule so `dfu-util` does not need `sudo`:

```bash
sudo tee /etc/udev/rules.d/45-stm32-dfu.rules > /dev/null <<'EOF'
SUBSYSTEM=="usb", ATTR{idVendor}=="0483", ATTR{idProduct}=="df11", MODE="0664", GROUP="plugdev"
SUBSYSTEM=="usb", ATTR{idVendor}=="26ac", MODE="0664", GROUP="plugdev"
EOF
sudo udevadm control --reload && sudo udevadm trigger
```
-->
::::

::::{tab-item} macOS

```bash
brew install dfu-util
```

```{note}
macOS does not need udev rules — `dfu-util` accesses USB devices directly via IOKit, without requiring `sudo`. Run the `dfu-util` commands in this guide **without** `sudo`.
```

::::
:::::


### Checkpoint ✅

```{testexpect}
Use `dfu-util` version `>= 0.9`. Older versions may silently truncate writes on STM32F4 targets. The Homebrew formula ships `0.11`, which is the version the procedure has been validated on. You can check the downloaded version with: 

```shell
dfu-util --version
---
```shell
dfu-util 0.11

Copyright 2005-2009 Weston Schmidt, Harald Welte and OpenMoko Inc.
Copyright 2010-2021 Tormod Volden and Stefan Schmidt
This program is Free Software and has ABSOLUTELY NO WARRANTY
Please report bugs to http://sourceforge.net/p/dfu-util/tickets/
```

## 2. Boot the FC in DFU Mode

- Remove power from your drone if it is powered on.
- Disconnect the USB cable connecting the Flight Controller to the Raspberry Pi.
- Reconnect it to your base station while keeping the **BOOT** button on the side of the flight controller pressed.

```{figure} ../_images/fc-setup/mamba-boot-button.png
:alt: mamba flight controller boot button location
:width: 50%
:name: mamba-boot-button

Mamba DD24-B flight controller BOOT button location.
```

- After a couple of seconds, release the BOOT button.
- Verify that the board is enumerated as an STM32 DFU device:

```bash
dfu-util -l
```

You should see one or more interfaces with the vendor/product ID `0483:df11` ("STMicroelectronics STM Device in DFU Mode"), as in the following image:

```{figure} ../_images/fc-setup/dfu-util-devices-list.png

Output of `dfu-util -l` with the FC in DFU mode.
```

## 3. Flash the PX4 Bootloader

Download the prebuilt `omnibusf4sd_bl` bootloader hex shipped with the PX4 user guide and convert it to a raw binary that `dfu-util` can flash. The conversion needs `arm-none-eabi-objcopy` from the ARM bare-metal toolchain:

:::::{tab-set}

::::{tab-item} Linux (Ubuntu)

```bash
sudo apt install binutils-arm-none-eabi   # provides arm-none-eabi-objcopy
curl -L -o omnibusf4sd_bl.hex \
  https://github.com/PX4/PX4-user_guide/raw/main/assets/flight_controller/omnibus_f4_sd/omnibusf4sd_bl_d52b70cb39.hex
arm-none-eabi-objcopy -I ihex -O binary omnibusf4sd_bl.hex omnibusf4sd_bl.bin
```

::::

::::{tab-item} macOS

```bash
brew install --cask gcc-arm-embedded   # provides arm-none-eabi-objcopy
curl -L -o omnibusf4sd_bl.hex \
  https://github.com/PX4/PX4-user_guide/raw/main/assets/flight_controller/omnibus_f4_sd/omnibusf4sd_bl_d52b70cb39.hex
arm-none-eabi-objcopy -I ihex -O binary omnibusf4sd_bl.hex omnibusf4sd_bl.bin
```

```{tip}
If you already have the PX4 dev toolchain installed via `brew tap PX4/px4 && brew install px4-dev`, you can skip the cask install — `arm-none-eabi-objcopy` is already on your `PATH`.
```

::::

:::::

```{tip}
The bootloader hash suffix `d52b70cb39` is tracked in the PX4 user guide; if the URL above 404s, re-resolve it from
[the PX4 bootloader-from-Betaflight page](https://docs.px4.io/main/en/advanced_config/bootloader_update_from_betaflight.html).
You can also build the bootloader from source with `make omnibusf4sd_bl` from a clone of [`PX4/PX4-Bootloader`](https://github.com/PX4/PX4-Bootloader).
```

With the FC still in DFU mode, flash the bootloader to address `0x08000000`:

```bash
dfu-util -a 0 --dfuse-address 0x08000000:leave -d 0483:df11 -D omnibusf4sd_bl.bin
```

```{tip}
Explanation of the `dfu-util` command:

* `-a 0`: selects the first DFU alternate setting (the internal flash).
* `--dfuse-address 0x08000000:leave`: writes to the start of flash and reboots the board into the freshly written image after the write completes.
* `-d 0483:df11`: matches the STM32 ROM DFU device.
* `-D omnibusf4sd_bl.bin`: the file to flash.
```

The command should yield the following output:

```{shell}
jacopotani@Jacopos-MacBook-Pro-2 ~ % dfu-util -a 0 --dfuse-address 0x08000000:leave -d 0483:df11 -D omnibusf4sd_bl.bin
dfu-util 0.11

Copyright 2005-2009 Weston Schmidt, Harald Welte and OpenMoko Inc.
Copyright 2010-2021 Tormod Volden and Stefan Schmidt
This program is Free Software and has ABSOLUTELY NO WARRANTY
Please report bugs to http://sourceforge.net/p/dfu-util/tickets/

dfu-util: Warning: Invalid DFU suffix signature
dfu-util: A valid DFU suffix will be required in a future dfu-util release
Opening DFU capable USB device...
Device ID 0483:df11
Device DFU version 011a
Claiming USB DFU Interface...
Setting Alternate Interface #0 ...
Determining device status...
DFU state(10) = dfuERROR, status(10) = Device's firmware is corrupt. It cannot return to run-time (non-DFU) operations
Clearing status
Determining device status...
DFU state(2) = dfuIDLE, status(0) = No error condition is present
DFU mode device DFU version 011a
Device returned transfer size 2048
DfuSe interface name: "Internal Flash  "
Downloading element to address = 0x08000000, size = 9612
Erase   	[=========================] 100%         9612 bytes
Erase    done.
Download	[=========================] 100%         9612 bytes
Download done.
File downloaded successfully
Submitting leave request...
Transitioning to dfuMANIFEST state
```


After the flash completes (this takes a few seconds), the board will reboot. It will now enumerate as a **PX4 bootloader** device under the `26AC` USB vendor ID (e.g., `26AC:0011`). Verify it has come back up:

:::::{tab-set}

::::{tab-item} Linux (Ubuntu)

```bash
lsusb                         # look for a 26AC:xxxx entry
ls /dev/serial/by-id/
```

```{figure} ../_images/fc-setup/lsusb-output.png
```

::::

::::{tab-item} macOS

`lsusb` is not shipped with macOS. Either install it with `brew install lsusb`, or use the built-in tools to enumerate USB and serial devices:

```bash
system_profiler SPUSBDataType | grep -A 3 -E "PX4|26AC"
ls /dev/tty.usbmodem*         # PX4 bootloader appears as e.g. /dev/tty.usbmodem01
```

```{note}
macOS does **not** populate `/dev/serial/by-id/`. The board is exposed only as `/dev/tty.usbmodem*` (and a matching `/dev/cu.usbmodem*`). It can take 1–2 seconds for the node to appear after the board reboots out of DFU.
```

::::

:::::

## 4. Flash the PX4 Firmware

Download the PX4 firmware binary for the `mamba-f405-mk2` target:

```bash
curl -L -O https://github.com/duckietown/PX4-Autopilot/releases/download/dd24-mamba-f405-mk2-v1.15.4-1/diatone_mamba-f405-mk2_default.bin
```

```{note}
Use **`dd24-mamba-f405-mk2-v1.15.4-1`** for the DD24 — this is the build on which the shipped `duckiedrone-px4-v2.params` file is known to load and save cleanly. The v2 hardware variant of the Mamba F405 MK2 ships **without** an on-board barometer or magnetometer; the param file restores `SYS_HAS_BARO=0`, `SYS_HAS_MAG=0`, `SYS_HAS_GPS=0`, and `CBRK_SUPPLY_CHK=894281` so preflight does not flag the missing sensors.

A newer `dd24-mamba-f405-mk2-v1.16.1-2` build also exists (with baro/mag drivers stripped at compile time) but currently has an unbisected param-related boot regression. Avoid it for now.
```

**Put the FC back into DFU mode (disconnect, hold BOOT, reconnect)**, confirm it shows up again in `dfu-util -l`, then flash the firmware to the application offset `0x08008000`:

```bash
dfu-util -a 0 --dfuse-address 0x08008000:leave -d 0483:df11 -D diatone_mamba-f405-mk2_default.bin
```

```{important}
The PX4 firmware is loaded **at offset `0x08008000`**, not at `0x08000000`. The first 32 KiB of flash is reserved for the bootloader you wrote in step 3. Writing the firmware to `0x08000000` would overwrite the bootloader.
```

After the flash completes, the board reboots and runs PX4. The boot sequence is: STM32 reset → PX4 bootloader at `0x08000000` → PX4 firmware at `0x08008000`.

````{tip}
**Alternative — use the PX4 serial uploader.**

Once the bootloader is flashed (step 3), you can also flash the firmware over USB-CDC with the PX4 uploader script. This is the canonical PX4 path — it verifies that the firmware's board ID matches the bootloader's reported board ID before erasing flash:

```bash
curl -L -O https://github.com/duckietown/PX4-Autopilot/releases/download/dd24-mamba-f405-mk2-v1.15.4-1/diatone_mamba-f405-mk2_default.px4
git clone --depth 1 https://github.com/PX4/PX4-Autopilot.git
python3 PX4-Autopilot/Tools/px4_uploader.py diatone_mamba-f405-mk2_default.px4
```

The `.px4` file is a JSON-wrapped, board-ID-tagged firmware envelope; `px4_uploader.py` verifies that its embedded board ID matches the bootloader's reported board (`42`) before erasing flash. The `dfu-util` flow above is preferred for fully programmatic deployments.
````

## 5. Connecting to the Flight Controller

```{attention}
Before you begin, **remove the propellers** and **disconnect the battery from the drone**.
```

```{todo}
We don't provide a Long USB cable in box, and the small USB cable wont allow for calibrating sensors
```

(qgroundcontrol-connection)=
### Installing QGroundControl and Restoring the correct parameters

By following these steps, you will be able to install QGroundControl, connect to your flight controller over USB, and configure your vehicle's parameters from a `.params` file.

```{note}
The shipped param file sets `EKF2_EV_CTRL = 0` so the EKF does not try to fuse vision before a VIO is online. Once a VIO publishes `VISION_POSITION_ESTIMATE` / `ODOMETRY` over MAVLink, raise `EKF2_EV_CTRL` to `7` (fuse vision pos + vel) or `15` (also fuse vision yaw — recommended on this magless airframe).
```

1. Install QGroundControl:
   - Go to the [QGroundControl website](http://qgroundcontrol.com/) and download the installer for your operating system (Windows, macOS, or Linux).
   - Follow the installation instructions for your OS:
     - **Windows**: Run the installer and follow the prompts.
     - **macOS**: Download the `.dmg` file, open it, and drag the QGroundControl icon into your Applications folder.
     - **Linux**: Follow the package manager or AppImage instructions provided on the QGroundControl download page.
   - Once installed, launch QGroundControl.

2. Connect to Your Vehicle over USB:
   - Connect a USB-C cable from your computer to the flight controller.
   - Open QGroundControl on your computer.
   - QGroundControl **auto-detects the flight controller over USB and connects automatically**.
   - Wait for a few moments; the top toolbar should show the vehicle as connected, then arrive at this summary page.

   ```{figure} ../_images/fc-setup/qgc-vehicle-setup.png
   ```

   - Here you can see many sections such as **Airframe** and **Sensors** are red, indicating that they need to be configured.

3. Open Parameters and load the `.params` file:

   ```{important}
   Import **exactly this file**: [`_static/duckiedrone-px4-v2.params`](../_static/duckiedrone-px4-v2.params)
   ```

   ```{todo}
   Remove the v1 / original file post v2 is found to be stable to foolproof
   ```

   - Click the **Parameters** tab from the left panel to view the configurable parameters for your vehicle.
   - In the Parameters screen, click on the **Tools** menu in the top right corner.
   - Select **Load from file for review…** from the dropdown menu.
   - Browse to the location of your `.params` file on your computer, select it, and click **Open**.
   - QGroundControl will load and apply the parameters from the file to your vehicle. There should be no errors during this step.

4. Reboot the Vehicle:
   - After loading the parameters, it is usually necessary to reboot the flight controller for changes to take effect.
   - You can reboot the vehicle by selecting **Reboot Vehicle** from the **Tools** menu.
   - After rebooting, reconnect to the vehicle. You will then see a summary page similar to the one below:

   ```{figure} ../_images/fc-setup/qgc-summary-post-params.png
   ```

5. Sensor Calibration:
   - Open the **Sensors** page from the Left Tab.
   - **Gyroscope:** start the gyroscope calibration and leave the drone still on a level surface until it completes.
   - **Accelerometer:** start the accelerometer calibration and hold the drone in the 6 different orientations requested by the on-screen prompts.
   - **Level Horizon:** leave the drone still on a level surface until this completes.
   - After the sensors are calibrated successfully, the **Sensors** tab should turn green.

   ```{figure} ../_images/fc-setup/qgc-summary-post-sensors.png
   ```

```{note}
The **Radio** page will stay red — this is expected. The Duckiedrone has no RC transmitter; the flight controller is commanded over MAVLink from the Raspberry Pi, so no radio configuration is needed.
```

```{todo}
Re-record the parameter-loading walkthrough video for PX4 (the previous Vimeo capture targeted ArduPilot/QGC).
```

### Additional Tips

- **Check for Errors:** The `duckiedrone-px4-v2.params` file loads cleanly in a single pass. If QGroundControl reports any parameter failing to load, you are on the wrong firmware build or using an outdated param file.
- **On-board calibration is mandatory:** the shipped `.params` file deliberately omits all `CAL_*` (accelerometer/gyro calibration) entries because those are tied to a specific board's sensor IDs. Run the **Sensors** calibration in QGroundControl on the actual flight controller after loading the parameters.

## Troubleshooting

```{trouble}
`dfu-util` shows no devices.
---
The FC has not entered DFU mode. Disconnect USB, hold the BOOT button while reconnecting, then run `dfu-util -l` again. On Linux, also confirm there is no kernel driver claiming the device (e.g., `ModemManager`) by checking `dmesg` after plug-in. On macOS, run `system_profiler SPUSBDataType | grep -i stm` and confirm the board enumerates as `STM32 BOOTLOADER` — if it does not, the BOOT button was released too early.
```

```{trouble}
On macOS, `dfu-util -l` lists the board but the write step hangs or errors with `Cannot open DFU device`.
---
Another process is holding the USB interface. The usual culprit is a previous `dfu-util` run that did not exit cleanly, or QGroundControl scanning for serial devices. Quit QGroundControl, then run `pkill -9 dfu-util` before retrying. Unlike Linux, macOS does **not** need any permissions or group changes for DFU access.
```

```{trouble}
After flashing, the board does not enumerate as a PX4 bootloader.
---
The most common cause is that the firmware was flashed to `0x08000000` instead of `0x08008000`, overwriting the bootloader. Re-enter DFU mode and re-run step 3 to restore the bootloader, then re-run step 4 with the correct address.
```

```{trouble}
`px4_uploader.py` reports "expected board ID 42, got X".
---
The wrong bootloader was flashed in step 3. Re-flash `omnibusf4sd_bl.bin` (board ID `42` matches the `mamba-f405-mk2` PX4 target).
```

```{trouble}
I am having issues following the instructions!
---
We're happy to help and hear your feedback! Please post a question on our Stack Overflow. You can find the instructions on how to join it [here](https://duckietown.slack.com/archives/CHHQJ0E0H/p1670874390660429).

You can also contact us via Slack in the following channel: [duckietown-sky-help](https://duckietown.slack.com/archives/CJWNCG667)
```
