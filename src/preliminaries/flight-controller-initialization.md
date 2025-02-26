```{seo}
:description: Instructions to initialize the flight controller on the Duckietown Duckiedrone model DD24. 
:keywords: Duckiedrone, Duckietown, autonomous drone, uav, flight controller, initialization
```

(dd24-fc-init)=
# Initializing the Flight Controller

```{needget}
* A base station computer
* Flight Controller
* USB to USB-C cable
---
* An up-to-date, initialized Flight Controller
```

The Flight Controller (FC) implements several low-level behaviors, e.g., stabilizing the Duckiedrone around roll, pitch, and yaw through three different PID controllers. Correctly configuring the Flight Controller is critical for flying safely.  

## Installing Dependencies and Flashing Ardupilot Firmware

### 1. Install dfu-util and screen:

- Connect the flight controller to a laptop running Ubuntu.
- Update the package list:

```bash
sudo apt update
```

- Install `dfu-util` and `screen`:

```bash
sudo apt install dfu-util
```

### 2. Entering DFU Mode:

- Disconnect the USB cable connecting the Flight Controller to the Raspberry Pi and reconnect it while keeping the BOOT button on the side of the flight controller pressed.\

```{figure} ../_images/fc-setup/speedybee-boot-button.png

SpeedyBee F405v3 BOOT button location.
```

- After a couple of seconds you can release the BOOT button.
- Execute the command `sudo dfu-util -l`, you should see a list of devices available to be flashed, as in the following image:

```{figure} ../_images/fc-setup/dfu-util-devices-list.png

List of devices in DFU mode output by `dfu-util`.
```

### 3. Download and Flash the Firmware:

Run the following command:

```bash
wget https://github.com/duckietown/duckiedrone-ardupilot-driver/blob/6f3bfa7fe7ad8ab06c829e1f915409ab7486f42f/assets/speedybeef4v3_bl.bin
```

Run `dfu-util` to flash the firmware:

```bash
sudo dfu-util -a 0 -s 0x08000000:leave -d 0483:df11 -D speedybeef4v3_bl.bin
```

```{tip}
Explanation of the `dfu-util` command:

* `sudo`: Grants administrative privileges to run the command.
* `dfu-util`: The name of the utility used for flashing firmware.
* `-a 0`: Selects the first DFU device (adjust if there are multiple).
* `-s 0x08000000:leave`: Sets the address and size for the new firmware.
* `-d 0483:df11`: Defines the vendor ID (0483) and product ID (df11) for Ardupilot devices.
* `-D speedybeef4v3_bl.bin`: Specifies the filename of the firmware to be flashed.
```

The flashing process might take a few minutes. Once finished, the flight controller will reboot.

### Install the Ardupilot firmware

You will need to flash a custom build of the Ardupilot firmware, it can be downloaded from [this link](https://github.com/duckietown/duckiedrone-ardupilot-driver/blob/6f3bfa7fe7ad8ab06c829e1f915409ab7486f42f/assets/arducopter.bin).

1. Start QGroundControl.
2. Go into the "Vehicle Setup" tab by first clicking on the QGroundControl logo.
      ![vehicle_setup](../_images/fc-setup/vehicle_setup.png)
3. Go into the "Firmware Flasher" tab by clicking on "Firmware" in the left sidebar.
      ![firmware_tab](../_images/fc-setup/firmware_tab.png)
4. Connect the Flight Controller through USB and select the `Ardupilot` option for the firmware, ticking `Advanced Settings` and then selecting `Custom firmware file...`. A file manager window will open to allow you to pick the `arducopter.bin` file you have just downloaded.
      ![firmware_selection](../_images/fc-setup/firmware_selection.png)

Here is a video summing up the process:

```{vimeo} 1045397898
```

## Starting the drone software stack

Now you will need to start the drone software stack, allowing you to connect to the flight controller from your laptop.

To do so you need to use the Duckietown shell `dts`.

````{attention}
Make sure that the `dts` on your laptop is:

- Running the `ente` distribution. You can check  by running `dts profile list`
- Updated to the latest version by running:

   ```bash
   dts update
   ```
````

To start the flight software stack execute the command

   ```bash
   dts duckiebot update -t duckiedrone --distro=ente -f [ROBOT_NAME]
   ```

Wait for the command to terminate before proceeding to the next step.

## Connecting to the Flight Controller

```{attention}
Unplug the battery from your drone!
```

(qgroundcontrol-connection)=
### Installing QGroundControl and Restoring the correct parameters

By following these steps, you will be able to install QGroundControl, connect to your flight controller via TCP, and restore your vehicle’s parameters from a `.param` file. You can download the correct `.param` file from [this link](https://raw.githubusercontent.com/duckietown/duckiedrone-ardupilot-driver/refs/heads/main/assets/speedybeef405_ardupilot.params).


1. Install QGroundControl:`
   - Go to the [QGroundControl website](http://qgroundcontrol.com/) and download the installer for your operating system (Windows, macOS, or Linux).
   - Follow the installation instructions for your OS:
     - **Windows**: Run the installer and follow the prompts.
     - **macOS**: Download the `.dmg` file, open it, and drag the QGroundControl icon into your Applications folder.
     - **Linux**: Follow the package manager or AppImage instructions provided on the QGroundControl download page.
   - Once installed, launch QGroundControl.

1. Connect to Your Vehicle via TCP:
   - Open QGroundControl on your computer.
   - Go to the **Comm Links** section by clicking on the **Q** application icon in the top left corner.
   - Select **Add** to create a new communication link.
   - Choose **TCP Link** from the dropdown.
   - Set the **Host Address** to `<robot_name>.local` and the **Port** to `5760`.
   - Click **Connect** to establish the connection with your flight controller.

1. Access the Vehicle Setup:
   - Once connected, open the menu by clicking on the by clicking on the **Q** application icon in the top left corner and open the **Vehicle Setup** page from the popup menu that appears.

1. Navigate to Parameters:
   - In the Vehicle Setup menu, select the **Parameters** tab to view the configurable parameters for your vehicle.

1. Load the `.param` File:
   - In the Parameters screen, click on the **Tools** menu in the top right corner.
   - Select **Load from file…** from the dropdown menu.
   - Browse to the location of your `.param` file on your computer, select it, and click **Open**.

1. Apply the Parameters:
   - QGroundControl will load and apply the parameters from the file to your vehicle. Progress indicators or messages will confirm that the parameters are being applied.

1. Reboot the Vehicle:
   - After loading the parameters, it is usually necessary to reboot the flight controller for changes to take effect.
   - You can reboot the vehicle by selecting **Reboot Vehicle** from the **Tools** menu.

Here is a video summing up how to restore the parameters after having connected to the Flight Controller:

```{vimeo} 1010195551
```

### Additional Tips

- **Multiple Loads:** Some parameters may require multiple loads until they are all applied correctly.
- **Check for Errors:** Ensure QGroundControl does not report any errors during the parameter loading process.

## Troubleshooting

```{trouble}
I am having issues following the instructions!
---
We’re happy to support and to hear your feedback! Please post a question on our StackOverflow, you can find the instructions on how to join it [here](https://duckietown.slack.com/archives/CHHQJ0E0H/p1670874390660429).

You can also contact us via Slack at the following channel: [duckietown-sky-help](https://duckietown.slack.com/archives/CJWNCG667)
```
