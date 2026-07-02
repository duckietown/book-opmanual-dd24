```{seo}
:description: Flash the Duckiedrone (DD24) ente image onto a Raspberry Pi 4 or Raspberry Pi 5 using dts init_sd_card.
:keywords: Duckiedrone, software initialization, SD card, flashing, Duckietown, dts, ente, Raspberry Pi 4, Raspberry Pi 5
```

(sw-initialization)=
(dd24-sw-init)=
# Software Initialization

```{needget}

* A computer (the “base station”) with an internet connection and the Duckietown Shell (`dts`) installed
* A microSD card (64 GB, U3, Class 10 recommended), e.g., the one from your Duckiedrone box
* A microSD card reader, e.g., the one from your Duckiedrone box
---

* A DD24 initialized and customized microSD card, ready for first boot
```

```{attention}
These instructions apply to **both** the Raspberry Pi 4 Model B and the Raspberry Pi 5. The same `ente` image is written through `dts init_sd_card`; only the underlying Raspberry Pi OS base layer differs and is chosen automatically at flashing time.

The legacy pre-built image for the Raspberry Pi 4 (`dt-amelia-DD24-brown2022-sd-card-*.zip` flashed through Balena Etcher) is no longer supported on the `ente` distribution. If you followed it before, re-flash with the procedure below.
```

## 1. Install the Duckietown Shell

On your base station, make sure the Duckietown Shell is installed and running on the `ente` profile. Follow the [laptop setup instructions](book-opmanual-duckiebot:laptop-setup) from the Duckiebot manual if you have not done this before.

Then confirm the profile and update:

```bash
dts profile list          # 'ente' must be the active profile
pipx upgrade duckietown-shell
dts update
```

## 2. Flash the SD card

1.  Insert the microSD card into your base station. Use the USB adapter from the Duckiedrone box if your base station does not have a microSD slot.

    ```{figure} ../_images/components-official/microSD_reader.png
    :width: 400px

    microSD card adapter
    ```

1.  Run `dts init_sd_card` with the `duckiedrone` type and the `DD24` configuration:

    ```bash
    dts init_sd_card \
        --hostname MY_ROBOT_NAME \
        --type duckiedrone \
        --configuration DD24 \
        --country US \
        --wifi miwifi:mypassword
    ```

    Flag reference:

    *   `--hostname` — the robot name. Follow the naming rules in the box below.
    *   `--type duckiedrone --configuration DD24` — picks the DD24 base image (Pi 4 and Pi 5 are both supported; the shell detects your target automatically).
    *   `--country` — two-letter country code where the drone will fly (required; Wi-Fi is disabled by default to comply with local regulations).
    *   `--wifi` — one or more networks in the form `ssid:psk`. Separate multiple networks with commas. This is the list the drone will try in client (CL) mode.

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

1.  When `dts init_sd_card` finishes, eject the SD card safely from the base station.

    ````{admonition} Eject your SD card safely.
    :class: warning

    Do not just unplug the SD card from the base station.

    ```{image} ../_images/rpi-sw-initialization/eject_sd.png
    :width: 300px
    ```
    ````

## 3. (Optional) Add extra Wi-Fi networks

`dts init_sd_card` writes a `wpa_supplicant.conf` file into the `config` partition of the SD card. To add networks later you can re-insert the SD card into your base station, open the `config` partition and append additional `network={...}` blocks.

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
