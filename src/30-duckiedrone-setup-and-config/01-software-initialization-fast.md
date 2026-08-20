```{seo}
:description: Flash the Duckiedrone DD24-B image to a microSD card with Balena Etcher for a quick, single-device setup.
:keywords: Duckiedrone, software initialization, SD card, flashing, Duckietown, dts, ente, Raspberry Pi 4, Raspberry Pi 5, quick start
```

```{needget}

* A computer (the “base station”) with an internet connection
* A microSD card (64 GB, U3, Class 10 recommended), e.g., the one from your Duckiedrone box
* A microSD card reader, e.g., the one from your Duckiedrone box
---

* An initialized Duckiedrone, ready for first boot
```

(dd24-sw-init-fast)=
# The Fast Way

This procedure consists of downloading a preset image and burning it to the microSD card using [Balena Etcher](https://etcher.balena.io/). The advantages of going the "fast" way are that:

- it works on any operating system
- all important configuration parameters are pre-set

The disadvantages of this procedure are:

- the Duckiedrone has default parameters, listed in the "cheatsheet" below, and the `robotname` cannot be changed
- since every robot on the network needs to have a unique `robotname`, this procedure should not be used if planning on using several Duckiedrones on the same network at the same time.

```{admonition} Cheatsheet
:class: note

Default robot name: `amelia`

Default `ssh` user name: `duckie`

Default `ssh` user password: `quackquack`

Default network configuration (robot will connect to this network after the first boot)

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

By downloading this image you are accepting the [Duckietown Software License](https://duckietown.com/sw-license/), [Terms and Conditions](https://duckietown.com/terms-and-conditions/) and [Privacy Policy](https://duckietown.com/privacy/), as well as robot configuration-specific licenses due to the presence of third-party software on the microSD card.
```

Download the latest Duckiedrone ente image to your base station:

```{button-link} https://duckietown-public-storage.s3.us-east-1.amazonaws.com/disk_image/duckietown-duckiedrone-dd24-b-ente-amelia-image-v2.zip
:color: primary
:shadow:

Download the DD24(-B) image from AWS
```

```{button-link} https://drive.google.com/file/d/1ziNpkxCpMd4B9EWIItZvr8deJgolN5JB/view?usp=sharing
:color: info
:shadow:

Download the DD24(-B) system from Google Drive
```

## 3. Flash the image to the microSD card

Connect the microSD card to the base station. Use the microSD-to-USB card reader if the base station does not have a microSD slot.

Open Balena Etcher and select the downloaded Duckiedrone image, then select the microSD card as the drive to flash. Finally, click the `"Flash"` button.

```{vimeo} 1202216734
:alt: microSD card flashing procedure for a DD24 using Balena Etcher
```

```{warning} **Double-check** that the "drive" is your microSD card.

You may be prompted to enter the base station password to proceed. This is normal: flashing a microSD card deletes everything that is on it, so Etcher is making sure this process is OK with you.
```

```{note}
Flashing will take 15 - 20 minutes.
```

## 3.1 (Ubuntu only) Country, Wi-Fi and hostname customization

```{attention}
This option works only prior to the [first boot](sec:first-boot).
```

If (and only if) you are on an Ubuntu machine, after flashing the microSD card it will mount three drives:

* `bootfs`: this partition contains important system files. Do not touch.
* `rootfs`: same as above, do not touch at this stage.
* `configfs`: this partition contains configuration files you can edit to customize important features of your Duckiedrone:
    - `hostname.txt`: this is the robotname, and the default is `amelia`. Keep in mind (a) this cannot be changed after the first boot, (b) there are [constraints on the naming](dd24-hostname-constraints).
    - `country.txt`: will contain the country [ISO 3166-1 alpha-2 code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2). default is `US`, change to your country if needed.
    - `wifi` folder, containing several files that allow for Wi-Fi network customization. You can edit `00-user.yaml` to add a custom Wi-Fi. Networks can be edited at any time after the first boot too. For additional information, refer to: [](dd24-network-config).

```{figure} ../_images/rpi-sw-initialization/dd24-b-image-partitions.png
:width: 80%
:alt: Configuration partition for the Duckiedrone microSD card
:name: dd24-b-image-partitions

Image configuration partition accessible through Ubuntu.
```

## 4. Proceed to the first boot

Safely eject the `bootfs` partition, then remove the microSD card from your base station and insert it into the Raspberry Pi's microSD card slot.

You are now ready for the [first boot](sec:first-boot).

```{note}
Through this approach, the Duckiedrone will boot searching for the default network. It is recommended to set up a `duckietown:quackquack` network before the first boot, or to connect the Duckiedrone to your router with an Ethernet cable. Once the first boot is complete, you can add or remove networks by following: [](dd24-network-config).
```

## Troubleshooting

````{trouble}
I am using a Mac and the flashing step fails for lack of permissions.
---

Go to your computer's `System Preferences > Security & Privacy > Files and Folders` and enable access to `Removable Volumes`.

![](../_images/rpi-sw-initialization/mac_troubleshooting.png)
````

```{trouble}
The Duckiedrone does not join my Wi-Fi after the first boot.
---

* Re-insert the microSD card into your base station and open the `configfs` partition.
* Check `country.txt`. Wi-Fi is disabled when the regulatory domain is unset, so it must contain the correct two-letter country code.
* Inspect `wifi/00-user.yaml`. Confirm that the network name and password are correct and that its YAML indentation uses spaces, not tabs.
* If you cannot access `configfs` from Ubuntu or are still stuck, use [](dd24-sw-init-adv) to flash the card with your network credentials.
```
