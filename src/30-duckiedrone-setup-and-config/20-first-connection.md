```{seo}
:description: Connect to a Duckiedrone DD24-B for the first time and check the connection from the base station.
:keywords: Duckiedrone first connection, DD24 network setup, Duckiedrone Dashboard, Wi-Fi configuration
```

```{needget}
- A live Duckiedrone: [](dd24-first-boot)

- A properly configured base station: [](dd24-initial-setup)

- A network that supports [mDNS](https://en.wikipedia.org/wiki/Multicast_DNS), or administrative access to the network

- (optional) Physical access to the network router, and an Ethernet cable
---
- A connected Duckiedrone
```

(dd24-first-connection)=
# First connection

The Duckiedrone is now ready for its first connection.

(dd24-how-to-connect)=
## Connecting to the Duckiedrone

Establishing a connection between the base station and the Duckiedrone is an essential step. There are several ways to establish a connection, with the preferred one being over Wi-Fi. To connect over Wi-Fi, both the Duckiedrone and the base station need to be connected to the same network.

### Duckiedrone Wi-Fi

The Duckiedrone automatically connects at boot to any known Wi-Fi network in range, including:

1. The network defined during the microSD card initialization procedure: [](dd24-sw-init)

2. The default backup network named `duckietown` with password `quackquack`

After the first boot, additional networks can be configured by following [](dd24-network-config).

<!--

::::{tab-set}

:::{tab-item} Client (CL) mode

Connect to the same network that the Duckiedrone is connected to if the Duckiedrone is in CL mode.

The default network is `duckietown` (password: `quackquack`)  

Once on the same network, SSH into the Duckiedrone using its robot name. If the robot is named `ROBOT_NAME`, run:

```bash
ssh duckie@ROBOT_NAME.local
```

replacing `ROBOT_NAME` with the actual name of the robot, e.g.:

```bash
ssh duckie@pdrone24.local
```

:::

:::{tab-item} Access Point (AP) mode

Connect to `duckietown-<hostname>-ap` if the Duckiedrone is in AP mode, where `<hostname>` is the robot name chosen during the initialization procedure.

If you forgot to change it, the default hostname is `amelia`.

:::

::::

## Accessing the Duckiedrone functionalities

```{admonition} Cheatsheet
:class: note

Default robot name: `amelia`

Default SSH user name: `duckie`

Default SSH user password: `quackquack`

SSH always possible: `ssh duckie@amelia.local`

**Default** access point (**AP**) network configuration:

- SSID: `duckietown-amelia-ap`

- Password: `quackquack`

**Default** client (**CL**) network configuration:

- SSID: `duckietown`

- Password: `quackquack`
```

-->

### Checkpoint ✅

When a connection is established, all four checks below pass. Run them in order.

`ROBOT_NAME` is the Duckiedrone name chosen during the [microSD card flashing procedure](dd24-sw-init). `ROBOT_IP` is the Duckiedrone IP address, shown in the `Address` column of `dts fleet discover` when available.

To verify that the Duckiedrone is on the same network as the base station:

````{testexpect}
On the base station, run:

```bash
dts fleet discover
```

Use Ctrl-C to stop the command.
---
The `ROBOT_NAME` row appears along with its status.
````

To verify that the base station and the Duckiedrone can communicate over the network:

````{testexpect}
On the base station, run:

```bash
ping ROBOT_NAME.local
```

Use Ctrl-C to stop the command.
---
A reply arrives for every packet, as in the example below.
````

````{admonition} A successful ping example
:class: note

```text
duckie@basestation ~ % ping amelia.local
PING amelia.local (192.168.0.81): 56 data bytes
64 bytes from 192.168.0.81: icmp_seq=0 ttl=64 time=41.965 ms
64 bytes from 192.168.0.81: icmp_seq=1 ttl=64 time=10.603 ms
64 bytes from 192.168.0.81: icmp_seq=2 ttl=64 time=10.582 ms
64 bytes from 192.168.0.81: icmp_seq=3 ttl=64 time=7.621 ms
64 bytes from 192.168.0.81: icmp_seq=4 ttl=64 time=25.420 ms
^C
--- amelia.local ping statistics ---
5 packets transmitted, 5 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 7.621/19.238/41.965/12.955 ms
```
````

If `ROBOT_NAME.local` does not resolve, commands that use that name will not work until name resolution is fixed. The network reachability of the Duckiedrone can still be tested with `ping ROBOT_IP`.

```{warning}
The network must support [mDNS](https://en.wikipedia.org/wiki/Multicast_DNS) to resolve `ROBOT_NAME.local`; mDNS is not required to reach the Duckiedrone by IP address. If name resolution is needed, ask whoever manages the network about mDNS on the subnet.
```

To verify that the Dashboard is reachable:

```{testexpect}
On the base station, open a browser and go to `http://ROBOT_NAME.local/`, or to `http://ROBOT_IP/` if the name does not resolve.
---
The Duckiedrone Dashboard loads.
```

The Dashboard can also be opened with `dts duckiebot dashboard ROBOT_NAME` if the name resolves, or `dts duckiebot dashboard ROBOT_IP` to connect by IP address. Commands that accept a target host also support `-H ROBOT_IP`.

The Dashboard provides access to many tools to manage the Duckiedrone. See [](dd24-dashboard-overview) for what each part of the Dashboard does.

To verify that Secure Shell (`ssh`) access works:

```{testexpect}
On the base station, run `ssh duckie@ROBOT_NAME.local`, or `ssh duckie@ROBOT_IP` if `.local` does not resolve. Enter the password set while preparing the microSD card.
---
A shell prompt on the Duckiedrone opens.
```

## Troubleshooting

```{trouble}
None of the connection checks work.
---
The most likely causes are:

- The base station and the Duckiedrone are not on the same network.

- The Duckiedrone [first boot procedure](dd24-first-boot) is not complete yet.

- `ROBOT_NAME.local` does not resolve through mDNS.

A general alternative that bypasses Wi-Fi, and can be useful during debugging, is connecting the Duckiedrone to the router with an Ethernet cable, if the router is physically accessible.
```

```{trouble}
The Duckiedrone answers `ping` at its IP address, but not at its hostname.
---
`mDNS` is unavailable on the network or is being filtered. Try a phone hotspot named `duckietown` with password `quackquack`, then reboot the Duckiedrone to isolate the network issue. If it works, ask the administrator of the original network to allow mDNS on the relevant subnet.
```

## Other notes on Duckiedrone networking (AP)

The Duckiedrone experimentally supports access point (AP) network configuration too, a setup in which it is the robot itself emitting the network, and the base station connecting to it. This feature is currently disabled until further testing is conducted. These instructions will be updated in due time to document this feature.

<!--
```{trouble}
I cannot connect to my Duckiedrone in AP mode.
---
Try using client mode and shut down the Docker container `dt-access-point` through the Portainer interface (accessible through your browser from your base station at `<hostname>.local:9000`).
```
-->
