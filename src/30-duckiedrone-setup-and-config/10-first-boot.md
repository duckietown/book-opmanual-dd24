```{seo}
:description: Learn how to perform the first boot of your Duckiedrone, establish a connection, and perform the first software update to ensure a smooth startup process.
:keywords: Duckiedrone first boot, dts duckiebot update, DD24-B setup, Raspberry Pi startup, Duckiedrone network configuration, robotics initialization, Duckiedrone setup
```

```{needget}

*   An initialized DD24 microSD card: [](dd24-sw-init)
---
*   A live `DD24`
```

(sec:first-boot)=
# First boot

There is only one first time you can connect to your Duckiedrone. Savor the experience.

## Before getting started

The first time a newly flashed microSD card is inserted into the Duckiedrone, a special "first boot" procedure is executed.

```{attention}
The first boot procedure will take roughly 10-15 minutes, during which your Raspberry Pi might look unresponsive. Do not interrupt the first boot procedure, e.g., by removing power to the Raspberry Pi. It will likely corrupt the microSD card. A corrupted microSD card will have to be flashed again.
```

During this process, the Duckiedrone will require a stable power source.

```{tip}
Make sure you have a wall outlet power adapter, e.g., a phone charger (5V, 2-3A) or a fully charged Duckiedrone battery before starting the process.
```

Do not power on the Raspberry Pi just yet.

## Getting started

To get started:

1. **Computer**: Make sure the Raspberry Pi is **not powered.**  

2. **Power**: Prepare a 5V, 2 or 3A wall adapter (preferred), or, if you have already assembled your Duckiedrone, alternatively a fully charged Duckiedrone battery.  

3. **Network**: Prepare a network connection to the Duckiedrone, for example by:
    - having a Wi-Fi network that matches the credentials preconfigured on the microSD card during the [initialization procedure](dd24-sw-init), or
    - connecting the Duckiedrone to the router your computer is connected to with an Ethernet cable.

4.  If you have not already, insert the initialized microSD card **into the card slot of the Raspberry Pi**.

    ```{attention}
    **Do not** connect the microSD card to a USB-A port of the Raspberry Pi through an adapter.
    ```

5.  **Power on the Raspberry Pi**: Insert the charger into the USB-C port of the Raspberry Pi, or plug in the Duckiedrone battery to the XT60 connector if the Duckiedrone is already assembled. You will see the Raspberry Pi's red and green LEDs turn on. The green LED shows computation usage and will blink so quickly that it appears solid green for several minutes.

    ````{seealso}
    ```{vimeo} 728539642
    :alt: DD24 first boot
    ```
    Duckiedrone first boot sequence.
    ````  

    Once the booting procedure is complete, the Duckiedrone will automatically connect to the default network, or any available network previously set up.

    You will be able to monitor the process through the `STATUS` column in `dts fleet discover` command output:

    ```bash
           |    Hardware    |    Type     | Model |  Status  |   Hostname  
    ------ | -------------- | ----------- | ----- | -------- | ------------
    amelia | raspberry_pi_64 | duckiedrone |  DD24 |  Ready   | amelia.local
    ```

    The `STATUS` field will read `BOOTING`, and then `READY` when the first boot process is about to finish.

    To confirm the first boot has completed successfully, [connect to your Duckiedrone](first_connection).

    After establishing the first connection, make sure to [update your Duckiedrone's software](dd24-software-update).

<!--
Watch a short video of a busy Raspberry Pi booting up for the first time: [Raspberry Pi first boot](https://vimeo.com/728539828/6cbc396872)
-->

<!--
    ::::{tab-set}

    :::{tab-item} Client (CL) mode

    Once the booting procedure is complete, the Duckiedrone will automatically connect to the default client network, or any available network previously set up in the `wpa_supplicant.conf` file.

    ```{seealso}
    A detailed guide on how to change `wpa_supplicant.conf` can be found in [](dd24-sw-init).
    ```

    ```{tip}
    If you already know the format of the `wpa_supplicant.conf` file, you can add different networks by manually editing it in the SD card’s `config` partition.

    To edit this file, you will need to:

    1. Power off the Duckiedrone
    1. Remove the SD card from the SD card slot of the Raspberry Pi
    1. Use the USB-A adapter to connect it back to the base station
    1. Open the `config` disk partition.
    ```

    :::

    :::{tab-item} Access Point (AP) mode

    Scan available networks through the base station: once the booting procedure is complete, you will find a network called `duckietown-<hostname>-ap`, where `<hostname>` is the name of the robot, as determined during the initialization procedure. The default name is `amelia`.

    ```{image} ../_images/first-boot/drone_wifi_ap.png
    :width: 300px
    ```

    :::

    ::::

-->

<!--
:::{trouble}
I disconnected my `P7` & `P11` pins but cannot see my robot on the network.
---
Sometimes things go awry during the first boot. It is possible that the Wi-Fi detection container times out. Search for a `duckietown-<hostname>-ap` network instead. Reboot the Duckiedrone (with disconnected pins) to have it join the configured existing network.
:::
-->

```{todo}
Verify AP mode works on DD24-B before uncommenting the instructions below.
```
<!--
Networks are typically one of the biggest headaches in robotics. We offer different network configurations to minimize these headaches. If you are not sure which choice to make, the right answer typically is: if you are in a university, go for AP mode. If you are at home, go for CL mode. In both cases, you need to place the jumper accordingly on your `P7` & `P11` pins before getting started.  

    ```{todo}
    Ditch this warning and update the images for DD24. (It should be already present on branch `daffy`)
    ```

    ```{attention}
    At present, AP mode is unstable. We suggest you use CL mode.
    ```

    ::::{tab-set}

    :::{tab-item} Client (CL) mode

    Use this mode to have the Duckiedrone connect to an existing local area network. This is the go-to choice if you have an existing network and admin access to it in the environment where you are operating.  

    1.  Pros:
        * both your base station and your Duckiedrone can talk with each other (and other devices on the network), and to the internet.  

    2.  Cons:
        * requires admin access to a pre-existing network in your space.  

    3.  How to:
        * __do not__ short pins `P7` & `P11` on the breadboard by __not__ using the provided jumper.

        ```{tip}
        Keep the jumper on _one_ pin to avoid misplacing and losing it!
        ```

        ```{figure} ../_images/first-boot/wifi_pins_not_shorted.jpg
        :width: 500px

        Pins `P7` & `P11` not shorted.
        ```
    :::

    :::{tab-item} Access Point (AP) mode

    Use this mode to have the Duckiedrone emit its own Wi-Fi network that your base station can connect to. This is the go-to choice if you do not have a network (WLAN) or admin access to the existing network where you are operating.  

    1.  Pros:

        *   You can connect to your Duckiedrone without the need for a pre-existing network infrastructure  

    2.  Cons:

        *   You might not have access to the internet (which is not required, but useful during development), unless your base station has a secondary network adapter (e.g., an Ethernet port) and you bridge the connection.  

    3.  How to:

        * __short__ pins `P7` & `P11` on the breadboard by using the provided jumper.

        ```{figure} ../_images/first-boot/wifi_pins_shorted.jpg
        :width: 500px

        Pins `P7` & `P11` shorted.
        ```

    :::
    ::::

-->

<!--
    ```{image} ../_images/first-boot/sd_card_insertion.png
    ```
```{todo}
update sd card image to DD24
```
-->
```{todo}
update first boot video DD24
```
