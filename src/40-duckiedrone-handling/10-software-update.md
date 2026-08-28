```{seo}
:description: Learn how to keep your Duckiedrone's onboard software up to date.
:keywords: Duckiedrone software, software update
```

```{needget}
- A base station on the same network as the Duckiedrone with Duckietown Shell (`dts`) installed

- A connected Duckiedrone: [](first_connection)

- Several minutes; the exact duration depends on the images to download and your network connection
---
- Updated Duckiedrone containers and base-station tools
```

(dd24-software-update)=
# How to update the software

Keep the Duckiedrone containers and the Duckietown tools on your base station up to date. These procedures update the base-station tools and pull the latest Duckiedrone containers for the active Duckietown Shell profile.

(handling-duckiedrone-software-update)=
## How to update a Duckiedrone

To pull the latest containers for your Duckiedrone, open a terminal on your base station and run:

```bash
dts duckiebot update ROBOT_NAME
```

Replace `ROBOT_NAME` with the hostname of your Duckiedrone, for example `amelia`. Wait for the command to finish; it can take several minutes, depending on the images that must be downloaded and your network connection.

```{attention}
Run this procedure after the [first boot](sec:first-boot).
```

(duckietown-desktop-update)=
## How to update your base station

Run these commands in order. First, update the Duckietown Shell (`dts`) itself:

```bash
pipx upgrade duckietown-shell
```

Then update the Duckietown Shell commands:

```bash
dts update
```

Finally, update the Duckietown desktop software:

```bash
dts desktop update
```
