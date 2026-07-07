```{seo}
:description: Flash the Duckiedrone (DD24) ente image onto a Raspberry Pi 4 or Raspberry Pi 5 using dts init_sd_card.
:keywords: Duckiedrone, advanced software initialization, SD card, flashing, Duckietown, dts, ente, Raspberry Pi 4, Raspberry Pi 5
```

```{todo}
Change https link to local reference after merging this book in the duckietown manual
```

```{needget}

* A computer (the “base station”) with an internet connection 
* A working Duckietown Shell (`dts`) installation: [Install the Duckietown Shell](https://docs.duckietown.com/ente/duckietown-manual/10-setup/02-software/duckietown-shell-dts-installation.html)
* A microSD card (64 GB, U3, Class 10 recommended), e.g., the one from your Duckiedrone box
* A microSD card reader, e.g., the one from your Duckiedrone box
* 10-20 minutes, depending on the internet connection
---

* An initialized Duckiedrone, ready for first boot
```

(dd24-sw-init-adv)=
# The "advanced" way

```{vimeo} 1207703689
:alt: sd card flashing procedure for a dd24 using dts init_sd_card
```

:::::{tab-set}
::::{tab-item} Ubuntu, macOS (native dts installation)

## Connect the SD card to the base station  

Insert the microSD card into your base station. Use the USB adapter from the Duckiedrone box if your base station does not have a microSD slot.

    ```{figure} ../_images/components-official-dd24/sd-card-reader.jpg
    :width: 40%
    :alt: Duckietown micro SD card adapter
    :name: duckietown USB to microUSB sd card reader

    Micro SD Card adapter
    ```

## 2. Flash the SD card

Run `dts init_sd_card` with the `duckiedrone` type and the `DD24` configuration:

```bash
dts init_sd_card \
    --hostname MY_ROBOT_NAME \
    --type duckiedrone \
    --configuration DD24 \
    --country US \
    --wifi mywifissid1:mypassword1[,mywifissid2:mypassword2,...]
```

    Where:

    *   `--hostname` — the robot name. Follow the naming rules in the box below.
    *   `--type duckiedrone --configuration DD24` — picks the DD24 base image.
    *   `--country` — two-letter country code where the drone will fly. A full list of codes can be found, e.g., on Wikipedia: [ISO 3166-1 alpha-2 codes](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2).
    *   `--wifi` — one or more networks in the form `ssid:psw`. Separate multiple networks with commas. 

Additional options are detailed on the [Duckietown Manual](https://docs.duckietown.com/ente/duckietown-manual/10-setup/03-duckiebot/flashing-sd-card-duckiebot-initialization-complete.html). 

```{todo}
Update link above when merging in the Duckietown manual
```

    ```{attention}
    The `hostname` **must** start with a lower-case letter and may contain **only** lower-case Latin letters and digits. Using special characters will break things and require re-flashing.

    *   ✅ `argo`
    *   ✅ `mydrone01`
    *   ❌ `mydrone_01`
    *   ❌ `My Drone`
    *   ❌ `Argo`

    The `hostname` **cannot** be changed after the first boot.
    ```

    ```{attention}
    If other Duckiedrones operate in the same environment, make sure your `hostname` is unique.
    ```


## 3. When `dts init_sd_card` finishes, eject the SD card safely from the base station.

    ````{admonition} Eject your SD card safely.
    :class: warning

    Do not just unplug the SD card from the base station.

    ```{image} ../_images/rpi-sw-initialization/eject_sd.png
    :width: 300px
    ```
    ````

::::
::::{tab-item} Duckietown Workspace

To initialize an SD card from within a Duckietown Workspace, start with the same procedure as if you were using Ubuntu, but instead of flashing to SD card, write the image to file and then flash it to SD card by using Balena Etcher through the host OS, similarly to the [fast way initialization procedure](dd24-sw-init-fast).


1. Run `dts init_sd_card` with the `duckiedrone` type and the `DD24` configuration:

```bash
dts init_sd_card \
    --hostname MY_ROBOT_NAME \
    --type duckiedrone \
    --configuration DD24 \
    --country US \
    --wifi mywifissid1:mypassword1[,mywifissid2:mypassword2,...]
```

When prompted to insert the SD card size, write any number, e.g., `64`. When prompted to list all possibilities say `y`, and then write the complete path to the image file name, e.g., `/image-file-name.img`. 

The `dts init_sd_card` process with proceed to create the image file. Finally, flash the `image-file-name-img` to your SD card through Balena Etcher on the host machine. 
::::
:::::


## 3. (Optional) Add extra Wi-Fi networks

`dts init_sd_card` writes a `wpa_supplicant.conf` file into the `config` partition of the SD card. To add networks later, you can re-insert the SD card into your base station, open the `config` partition, and append additional `network={...}` blocks.

```bash
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US

network={
  id_str="network_1"
  ssid="duckietown"
  psk="quackquack"
  key_mgmt=WPA-PSK
}

network={
  id_str="network_2"
  ssid="example-second-network"
  psk="quackquack2"
  key_mgmt=WPA-PSK
}
```

*   `country` — change if you are not in the US (e.g., `CH` for Switzerland, `CA` for Canada; [full list](https://www.arubanetworks.com/techdocs/InstantWenger_Mobile/Advanced/Content/Instant%20User%20Guide%20-%20volumes/Country_Codes_List.htm) of country codes).
*   `id_str` — unique identifier for each network entry.
*   `ssid` / `psk` — the network name and password.

```{note}
This file can also be edited after the first boot if you want to add other networks.
```

You are now ready for the [first boot](sec:first-boot).

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
