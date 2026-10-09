```{seo}
:description: Update settings on a previously initialized Duckiedrone DD24 microSD card without reflashing it.
:keywords: Duckietown, Duckiedrone, DD24, microSD card, SD card, update, dts sd_card update, Wi-Fi
```

```{needget}
- A base station with a working Duckietown Shell (`dts`) installation

- A previously initialized Duckiedrone DD24 microSD card

- A microSD card reader

- A broadband internet connection

- `10-20 min`, depending on the internet connection
---
- A Duckiedrone DD24 microSD card with updated settings
```

(dd24-sd-card-update)=
(dd24-update-initialized-sd-card)=
# Updating an existing microSD card

`dts sd_card update` changes the settings stored on an initialized microSD card without reflashing it. The command works on the card itself: power off the Duckiedrone, remove the microSD card, and connect it to the base station through the card reader.

To update an initialized Duckiedrone DD24 microSD card, run the following command. `HOSTNAME` is the new robot name, `WIFI` is a comma-separated list of Wi-Fi networks, and `COUNTRY` is the new two-letter Wi-Fi country code:

```shell
dts sd_card update --type duckiedrone --configuration DD24 [--hostname HOSTNAME] [--wifi WIFI] [--country COUNTRY] [--password]
```

At least one of `--hostname`, `--wifi`, `--country`, or `--password` is required. DTS changes only the settings supplied on the command line. Some disk images store Wi-Fi and country settings together, in which case both `--wifi` and `--country` must be provided in the same command.

For example, to replace the Wi-Fi networks and the country code in one command:

```shell
dts sd_card update --type duckiedrone --configuration DD24 --wifi mywifissid1:mypassword1,mywifissid2:mypassword2 --country US
```

`--password` prompts for a new password for the `duckie` account and for its confirmation. It must contain at least eight characters and cannot contain colons or line breaks. The characters typed are not displayed. A password update is applied when the Duckiedrone next boots. If DTS reports that the disk image does not support setting a password, use a current supported image version.

```{attention}
Updating the `WIFI` configuration replaces the existing data.
```

Without `--device DEVICE`, DTS prompts for the physical microSD card device. Specify it to select the device directly.

To see all available options, run:

```shell
dts sd_card update --help
```
