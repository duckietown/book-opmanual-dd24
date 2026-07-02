```{seo}
:description: Learn how to perform the first boot of your Duckiedrone (DD24), configure network settings, and ensure a smooth startup process.
:keywords: Duckiedrone first boot, DD24 setup, Raspberry Pi startup, Duckiedrone network configuration, robotics initialization, Duckietown drone setup
```

(sec:first-boot)=
# First boot

There is only one first time you can connect to your Duckiedrone. Savor the experience.

```{needget}

*   An assembled `DD24`
*   A `DD24` initialized SD card, see [](sw-initialization)
---

*   A live `DD24`
```

## Before getting started

The first time a newly flashed SD card is inserted in the Duckiedrone a special “first boot” procedure is executed.

```{note}
The first boot procedure will take roughly 10-15 minutes, during which your Raspberry Pi might look unresponsive. It is crunching as fast as it can, do not worry.
```

During this process the Duckiedrone will require a stable power source.

```{attention}
Make sure you have a wall outlet power adapter, e.g., a phone charger (5V, 2-3A) or a fully charged Duckiedrone battery before starting the process.
```

Do not power the Raspberry Pi just yet.

## Getting started

```{warning}
Do not interrupt the first boot procedure, e.g., by removing power to the Raspberry Pi. It will likely corrupt the SD card. A corrupted SD card will have to be flashed again.
```

1.  Make sure the Raspberry Pi is **not powered.**  
    
2.  Make sure you have a wall adapter or fully charged battery available.  
    
3.  Networks are typically one of the biggest headaches in robotics. We offer different network configurations to minimize these headaches. If you are not sure which choice to make, the right answer typically is: if you are in a university go for AP mode. If you are at home go for CL mode. In both cases, you need to place the jumper accordingly on your `P7` & `P11` pins before getting started.  
    
    ```{todo}
    Ditch this warning and update the images for DD24. (It should be already present on branch `daffy`)
    ```

    ```{attention}
    At present AP mode is unstable, we suggest you to use CL mode.
    ```

    ::::{tab-set}

    :::{tab-item} Client (CL) mode      
    
    If you want to have the drone connect to an existing local area network. This is the go-to choice if you have an existing network and admin powers to it in the environment where you are operating.  

    1.  Pros:
        * both your base station and your drone can talk with each other (and other devices on the network), and to the internet.  

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
    
    If you want to have the drone emit its own Wi-Fi network that your base station can connect to. This is the go-to choice if you do not have a network (WLAN) or admin access to the existing network where you are operating.  
        
    1.  Pros:
            
        *   You can connect to your drone without the need for a pre-existing network infrastructure  
                
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

4.  If you haven't already, insert the initialized microSD card inside the microSD card slot of the Raspberry Pi, as shown [here](attach_pi_hat).
    
    ```{attention}
    **Do not** connect the SD card inside the adapter to a USB-A port of the Raspberry Pi. 
    ```

    ```{image} ../_images/first-boot/sd_card_insertion.png
    ```

5.  Power the Raspberry Pi
    
    *  You will see a red and green light turning on the Raspberry Pi. The green light shows computation usage. You should expect it to become solid green for several minutes.  
        
        ```{seealso}
        Watch a short video of a busy Raspberry Pi booting up for the first time: [Raspberry Pi first boot](https://vimeo.com/728539828/6cbc396872)
        ```  
            
6.  Wait until the first boot sequence is complete:

    ::::{tab-set}

    :::{tab-item} Client (CL) mode      
    
    Once the booting procedure is complete the Duckiedrone will automatically connect to the default client network, or any available network previously set up in the `wpa_supplicant.conf` file.
    
    ```{seealso}
    A detailed guide on how to change `wpa_supplicant.conf` can be found in [](sw-initialization).
    ```

    ```{tip}    
    If you already know the format of the `wpa_supplicant.conf` file, you can add different networks by manually editing it in the SD card’s `config` partition. 

    To edit this file, you will need to:

    1. Power off the drone
    1. Remove the SD card from the SD card slot of the raspberry pi
    1. Use the USB-A adapter to connect it back to the base station
    1. Open the `config` disk partition.
    ```

    :::
    
    :::{tab-item} Access Point (AP) mode
    
    Scan available networks through the base station: once the booting procedure is complete you will find a network called `duckietown-<hostname>-ap`, where `<hostname>` is the name of the robot, as determined during the initialization procedure. The default name is `amelia`.

    ```{image} ../_images/first-boot/drone_wifi_ap.png
    :width: 300px
    ```
 
    
    :::

    ::::


Congratulations, you are now ready to connect to your Duckiedrone for the first time!


:::{trouble}
I disconnected my `P7` & `P11` pins but cannot see my robot on the network.
---
Sometimes things go awry during the first boot. It is possible that the Wi-Fi detection container times out. Search for a `duckietown-<hostname>-ap` network instead. Reboot the Duckiedrone (with disconnected pins) to have it join the configured existing network.
:::
