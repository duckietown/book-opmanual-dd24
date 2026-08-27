```{seo}
:description: Flash the Duckiedrone DD24-B ente image onto a Raspberry Pi 4 or Raspberry Pi 5 using dts init_sd_card.
:keywords: Duckiedrone, advanced software initialization, SD card, flashing, Duckietown, dts, ente, Raspberry Pi 4, Raspberry Pi 5
```

```{todo}
Change https link to local reference after merging this book in the Duckietown manual.
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
# The Complete Way

```{vimeo} 1207703689
:alt: microSD card flashing procedure for a DD24 using `dts init_sd_card`
```

## Connect the microSD card to the base station

Insert the microSD card into your base station. Use the USB adapter from the Duckiedrone box if your base station does not have a microSD slot.

```{figure} ../_images/components-official-dd24/sd-card-reader.jpg
:width: 40%
:alt: Duckietown microSD card adapter
:name: duckietown USB to microUSB sd card reader
microSD card adapter
```

## Create the image

Before continuing, confirm that `ente` is the active Duckietown Shell profile:

```bash
dts profile list
```

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
*   `--country` — two-letter country code where the Duckiedrone will fly. A full list of codes can be found, e.g., on Wikipedia: [ISO 3166-1 alpha-2 codes](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2).
*   `--wifi` — one or more networks in the form `ssid:psw`. Separate multiple networks with commas.

Additional options are detailed on the [Duckietown Manual](https://docs.duckietown.com/ente/duckietown-manual/10-setup/03-duckiebot/flashing-sd-card-duckiebot-initialization-complete.html).

```{todo}
Update link above when merging in the Duckietown manual.
```

(dd24-hostname-constraints)=
### Duckiedrone hostname naming constraints
```{attention}
The `hostname` **must** start with a lower-case letter and may contain **only** lower-case Latin letters and digits. Using special characters will break things and require re-flashing.

*   ✅ `argo`
*   ✅ `mydrone01`
*   ❌ `mydrone_01`
*   ❌ `My Duckiedrone`
*   ❌ `Argo`

The `hostname` **cannot** be changed after the first boot.
```

```{attention}
If other Duckiedrones operate in the same environment, make sure your `hostname` is unique.
```

## Flashing the image

After downloading the base image, `dts init_sd_card` will prompt you to select the target drive to flash the image to.

:::::{tab-set}
::::{tab-item} Ubuntu, macOS (native dts installation)

On Ubuntu or macOS with a native DTS installation, enter the nominal microSD card capacity (e.g., `64` for 64GB), and a list of available devices will appear. Select your microSD card and confirm your choice.

Once the flashing is complete, safely eject the microSD card from the base station.

````{admonition} Eject your microSD card safely.
:class: warning

Do not just unplug the microSD card from the base station.

```{image} ../_images/rpi-sw-initialization/eject_sd.png
:width: 300px
```
````

::::
::::{tab-item} Duckietown Workspace

Inside a Duckietown Workspace the system will not be able to detect your microSD card. Therefore, you will flash the image to a file, then burn it to a microSD card with Balena Etcher through the host OS, similarly to the [fast way initialization procedure](dd24-sw-init-fast).

When prompted to enter the microSD card capacity, write any number, e.g., `64`. When prompted to list all possibilities, enter `y`, then write the complete path to the image file, e.g., `/image-file-name.img`.

The `dts init_sd_card` process will proceed to create the image file.

Once complete, switch to the host machine, open Balena Etcher, and flash `image-file-name.img` to your microSD card.
::::
:::::

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
The Duckiedrone does not join my Wi-Fi after the first boot.
---

*   Double-check the `--country` flag you passed to `dts init_sd_card`. Wi-Fi is disabled by default if the regulatory domain is unset.
*   Double-check your network credentials.
*   If you are still stuck, flash again with the correct flags shown above.
```
