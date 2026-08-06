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


````{admonition} Successful bootloader flashing output (MacOS)
:class: dropdown

```
user@MacBook-Pro-2 ~ % dfu-util -a 0 --dfuse-address 0x08000000:leave -d 0483:df11 -D omnibusf4sd_bl.bin
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
````

````{admonition} Successful bootloader flashing output (Ubuntu)
:class: dropdown

```
jatinvira@ultron:~$ dfu-util -a 0 --dfuse-address 0x08000000:leave -d 0483:df11 -D omnibusf4sd_bl.bin
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
````

After the flash completes (this takes a few seconds), the board will reboot. It will now enumerate as a **PX4 bootloader** device under the `26AC` USB vendor ID (e.g., `26AC:0011`). Verify it has come back up:

:::::{tab-set}

::::{tab-item} Linux (Ubuntu)

```bash
lsusb                         # look for a 26AC:xxxx entry
ls /dev/serial/by-id/
```

```{figure} ../_images/fc-setup/lsusb-output.png
:alt: lsusb output on Ubuntu after successfull bootloader flashing
:width: 90%
:name: ubuntu-lsusb-output

Example successful `lsusb` output on Ubuntu
```

::::

::::{tab-item} macOS

`lsusb` is not shipped with macOS. You can use the built-in tools to enumerate USB and serial devices:

```bash
system_profiler SPUSBDataType | grep -A 3 -E "PX4|26AC"
ls /dev/tty.usbmodem*         # PX4 bootloader appears as e.g. /dev/tty.usbmodem01
```

```{note}
macOS does **not** populate `/dev/serial/by-id/`. The board is exposed only as `/dev/tty.usbmodem*` (and a matching `/dev/cu.usbmodem*`). It can take 1–2 seconds for the node to appear after the board reboots out of DFU.
```

```{todo [DTSW-8047]}
PX4 bootlader installer - `lsusb` approach does not work on MacOS 
```

::::

:::::

## 4. Flash the PX4 Firmware

Download the PX4 firmware binary for the `mamba-f405-mk2` target:

```bash
curl -L -O https://github.com/duckietown/PX4-Autopilot/releases/download/dd24-mamba-f405-mk2-v1.15.4-1/diatone_mamba-f405-mk2_default.bin
```

```{note}
Use **`dd24-mamba-f405-mk2-v1.15.4-1`** for the DD24 — this is the build on which the shipped `duckiedrone-px4-v3.params` file is known to load and save cleanly. The v2 hardware variant of the Mamba F405 MK2 ships **without** an on-board barometer or magnetometer; the param file restores `SYS_HAS_BARO=0`, `SYS_HAS_MAG=0`, `SYS_HAS_GPS=0`, and `CBRK_SUPPLY_CHK=894281` so preflight does not flag the missing sensors.

A newer `dd24-mamba-f405-mk2-v1.16.1-2` build also exists (with baro/mag drivers stripped at compile time) but currently has an unbisected param-related boot regression. Avoid it for now.
```

* **Put the FC back into DFU mode (disconnect, hold BOOT, reconnect)**, 
* confirm it shows up again in `dfu-util -l`, then 
* flash the firmware to the application offset `0x08008000`:

   ```bash
   dfu-util -a 0 --dfuse-address 0x08008000:leave -d 0483:df11 -D diatone_mamba-f405-mk2_default.bin
   ```

````{admonition} Successful PX4 firmware flashing output (MacOS)
:class: dropdown

```
user@MacBook-Pro-2 ~ % dfu-util -a 0 --dfuse-address 0x08008000:leave -d 0483:df11 -D diatone_mamba-f405-mk2_default.bin
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
Downloading element to address = 0x08008000, size = 968296
Erase   	[=========================] 100%       968296 bytes
Erase    done.
Download	[=========================] 100%       968296 bytes
Download done.
File downloaded successfully
Submitting leave request...
Transitioning to dfuMANIFEST state
```
````

````{admonition} Successful PX4 firmware flashing output (Ubuntu)
:class: dropdown

```
jatinvira@ultron:~$ dfu-util -a 0 --dfuse-address 0x08008000:leave -d 0483:df11 -D diatone_mamba-f405-mk2_default.bin
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
Downloading element to address = 0x08008000, size = 968296
Erase   	[=========================] 100%       968296 bytes
Erase    done.
Download	[=========================] 100%       968296 bytes
Download done.
File downloaded successfully
Submitting leave request...
Transitioning to dfuMANIFEST state
```
````

```{important}
The PX4 firmware is loaded **at offset `0x08008000`**, not at `0x08000000`. The first 32 KiB of flash is reserved for the bootloader you wrote in step 3. Writing the firmware to `0x08000000` would overwrite the bootloader.
```

After the flash completes, the board reboots and runs PX4. The boot sequence is: STM32 reset → PX4 bootloader at `0x08000000` → PX4 firmware at `0x08008000`.


```{todo}
Test this alternative part or remove
```

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
