(dd24-sd-card-update)=
# Updating an existing microSD card

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

(dd24-update-initialized-sd-card)=
## Updating an initialized microSD card

To update an initialized Duckiedrone DD24 microSD card, run the following command. `HOSTNAME` is the new robot name, `WIFI` is a comma-separated list of Wi-Fi networks, and `COUNTRY` is the new two-letter Wi-Fi country code:

```shell
dts sd_card update --type duckiedrone --configuration DD24 [--hostname HOSTNAME] [--wifi WIFI] [--country COUNTRY]
```

At least one of `--hostname`, `--wifi`, or `--country` is required. DTS changes only the settings you supply. Some disk images store Wi-Fi and country settings together, in which case you must provide both `--wifi` and `--country` in the same command.

```{attention}
Updating the `WIFI` configuration replaces the existing data.
```

If you omit `--device DEVICE`, DTS prompts you to select the physical microSD-card device. Specify it to select the device directly.

To see all available options, run:

```shell
dts sd_card update --help
```
