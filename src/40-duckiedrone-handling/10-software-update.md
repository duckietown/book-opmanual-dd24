```{seo}
:description: Learn how to keep your Duckiedrone's onboard software up to date.
:keywords: Duckiedrone software, software update
```

```{needget}

* A connected Duckiedrone: [](first_connection)
* 10-15 minutes
---
* An up-to-date Duckiedrone
```

(dd24-software-update)=
(duckietown-software-update)=
# How to update the software

Keep the software on the Duckiedrone and your base station up to date. These procedures synchronize the software stacks on both devices with the latest version.

(handling-duckiedrone-software-update)=
## How to update a Duckiedrone

To update the software on your Duckiedrone, open a terminal on your base station and run:

```bash
dts duckiebot update ROBOT_NAME
```

Replace `ROBOT_NAME` with the hostname of your Duckiedrone, for example `amelia`. This process should take roughly 10-15 minutes.

```{attention}
Run this procedure after the [first boot](sec:first-boot).
```

(duckietown-desktop-update)=
## How to update your base station

To update the Duckietown Shell (`dts`) itself, run:

```bash
pipx upgrade duckietown-shell
```

To update the Duckietown Shell (`dts`) commands, run:

```bash
dts update
```

To update the Duckietown desktop software, run:

```bash
dts desktop update
```
