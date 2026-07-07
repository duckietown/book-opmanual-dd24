```{seo}
:description: 
:keywords: Duckiedrone, software initialization, SD card, flashing, Duckietown, dts, ente, Raspberry Pi 4, Raspberry Pi 5, quick start
```
cd /etc/NetworkManager/system-connections/
```{needget}

* A computer (the “base station”) with an internet connection
* A microSD card (64 GB, U3, Class 10 recommended), e.g., the one from your Duckiedrone box
* A microSD card reader, e.g., the one from your Duckiedrone box
---

* An initialized Duckiedrone, ready for first boot
```

(dd24-sw-init-fast)=
# The "fast" way

This procedure consists of downloading a preset image, and burning it to the SD card using [Balena Etcher](https://etcher.balena.io/). The advantages of going the "fast" way are that: 

- it works on any operating system
- all important configuration parameters are pre-set

The disadvantages of this procedure are: 

- the Duckiedrone has default parameters, listed in the "cheatsheet" below, and the `robotname` cannot be changed
- since every robot on the network needs to have a unique `robotname`, this procedure should not be used if planning on using several Duckiedrones on the same network at the same time. 

```{note}
```

```{admonition} Cheatsheet
:class: note

Default robot name: `amelia`

Default `ssh` user name: `duckie`

Default `ssh` user password: `quackquack`

Defaul network configuration (robot will connect to this network after the first boot)

*   SSID: `duckietown`    
*   Password: `quackquack`
```

<!--
**Default** access point (**AP**) network configuration:

*   SSID: `duckietown-amelia-ap`
*   Password: `quackquack`
    

**Default** client (**CL**) network configuration:
-->

## 1. Install Balena Etcher

Balena Etcher is a program to flash disk images to drives. 

Download and install the version appropriate for your operating system from [Balena Etcher's website](https://etcher.balena.io/#download-etcher).

## 2. Download the Duckietown image


```{admonition} Legalities
:class: note

By downloading this image you are accepting the [Duckietown Software License](https://duckietown.com/sw-license/), [Terms and Conditions](https://duckietown.com/terms-and-conditions/) and [Privacy Policy](https://duckietown.com/privacy/), as well as robot configuration-specific licenses due to the presence of third party software in the SD card. 
```

Download the latest drone ente image to your base station:

```{button-link} https://duckietown-public-storage.s3.us-east-1.amazonaws.com/disk_image/dd24-b-ente-amelia-v1.img.zip
:color: primary
:shadow:

Download the DD24(-B) image from AWS 
```

```{button-link} https://drive.google.com/file/d/1WpuhmfQ9VxKz07QH2FMNfZ0TsFz0eC90/view?usp=sharing
:color: info
:shadow:

Download the DD24(-B) system from Google Drive
```



## 3. Flash the image to the SD card

Connect the microSD card to the base station. Use the micro SD to USB card reader if the base station does not have a micro SD port.
  
```{figure} ../_images/components-official-dd24/sd-card-reader.jpg
:width: 40%
:alt: Duckietown micro SD card adapter
:name: duckietown USB to microUSB sd card reader

Micro SD Card adapter
```

Open Balena Etcher and select the downloaded drone image, then select the micro SD card as the drive to flash. Finally, click the `"Flash"` button.


```{vimeo} 1202216734
:alt: sd card flashing procedure for a dd24 using balena etcher
```


```{warning} **Double check** that the "drive" is your micro SD card.

You may be prompted to enter the base station password to proceed. This is normal: flashing an SD card deletes everything that is on it, so Etcher is making sure this process is OK with you.
```

```{note} Flashing will take 15 - 20 minutes.
```

## 4. Proceed to the first boot

Remove the SD card from your base station and insert it in the SD card adapter of your Raspberry Pi. You are now ready for the [first boot](sec:first-boot). 

```{note}
Through this approach, the Duckiedrone will boot searching for the default network. It is recommended to set up a `duckietown:quackquack` network before the first boot, or to connect the Duckiedrone to your router with an ethernet cable. Once the first boot is complete, you can add or remove networks by following: [](dd24-network-config).  
```

## Troubleshooting

````{trouble}
I am using a Mac and the flashing step fails for lack of permissions.
---

Go to your computer's `System Preferences > Security & Privacy > Files and Folders` and enable access to `Removable Volumes`.

![](../_images/rpi-sw-initialization/mac_troubleshooting.png)
````


```{trouble}
`dts init_sd_card` fails with "unknown robot type duckiedrone".
---

Your Duckietown Shell is out of date or the wrong profile is active. Run:

    dts profile list          # 'ente' must be the active profile
    pipx upgrade duckietown-shell
    dts update

then rerun the `dts init_sd_card` command.
```


```{trouble}
The drone does not join my Wi-Fi after the first boot.
---

*   Double-check the `--country` flag you passed to `dts init_sd_card`. Wi-Fi is disabled by default if the regulatory domain is unset.
*   Re-insert the SD card into your base station and inspect `config/wpa_supplicant.conf`. Confirm that your `ssid` and `psk` are correct and that the file has Unix line endings.
*   If you are still stuck, flash again with the correct flags (see [](first_connection)).
```