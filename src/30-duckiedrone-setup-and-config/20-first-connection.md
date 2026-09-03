```{seo}
:description: Learn how to connect to your Duckiedrone (DD24-B).
:keywords: Duckiedrone first connection, DD24 network setup, Duckiedrone Dashboard, Wi-Fi configuration
```

```{needget}
- A live Duckiedrone: [](sec:first-boot)

- A properly configured Duckiedrone: [](dd24-network-config)

- A properly configured base station: [](dd24-initial-setup)

- A network that supports [mDNS](https://en.wikipedia.org/wiki/Multicast_DNS), or for which you have administrative access

- (optional) Physical access to the network router, and an Ethernet cable
---
- A connected Duckiedrone
```

(first_connection)=
# First connection

You are now ready to connect to your Duckiedrone.

(dd24-how-to-connect)=
## Connecting to the Duckiedrone  

Establishing a connection between the base station and the Duckiedrone is an essential step. There are several ways to establish a connection, with the preferred one being over Wi-Fi. To connect over Wi-Fi, both the Duckiedrone and your base station need to be connected to the same network.

### Duckiedrone Wi-Fi

The Duckiedrone automatically connects at boot to any known Wi-Fi network in range, including:

1. The network defined during the microSD card initialization procedure: [](dd24-sw-init)

2. The default backup network named `duckietown` with password `quackquack`

After the first boot, you can configure additional networks by following [](dd24-network-config).

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

### Testing the connection

When a successful connection is established, all the following will work:

#### Duckietown fleet discovery tool

Open a terminal on the base station and run:

```bash
dts fleet discover
```

If the robot is on the same network, it will appear along with its status.

#### `ping`

To test if your computer and Duckiedrone are able to communicate over the network, open a terminal and:

```bash
ping <hostname>.local
```

````{admonition} A successful ping example
:class: note

```bash
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

where `<hostname>` is the robot name chosen during the [microSD card flashing procedure](dd24-sw-init).

If `<hostname>.local` does not resolve, commands that use that hostname will not work until name resolution is fixed. You can still test network reachability with the Duckiedrone's IP address.

```{warning}
The network must support [mDNS](https://en.wikipedia.org/wiki/Multicast_DNS) to resolve `<hostname>.local`; mDNS is not required to ping the Duckiedrone's IP address. Commands that use `<hostname>.local` depend on mDNS. Availability varies by network, so ask your network administrator to enable mDNS on the subnet used by your class or lab.
```

#### The Dashboard

You can access the robot's Dashboard at `http://<hostname>.local`, or through the command `dts duckiebot dashboard <hostname>`.

Accessing the Duckiedrone's Dashboard provides access to many tools to manage the Duckiedrone.

```{todo [DTSW-8178]}
Create a dedicated Duckiedrone dashboard explanation page.
```

#### Secure Shell (`ssh`)

You can `ssh` into the Duckiedrone with `ssh duckie@<hostname>.local`, using the default password `quackquack`.

## Troubleshooting

If any of these basic interfacing commands are not working, the most likely causes are:

- Your computer and Duckiedrone are not on the same network.

- The Duckiedrone's [first boot procedure](sec:first-boot) is not complete yet.

- `<hostname>.local` does not resolve through mDNS.

A general alternative networking solution that bypasses Wi-Fi, and can be useful during debugging, is connecting the Duckiedrone via an Ethernet cable to the router, if you have physical access to it.

```{trouble}
I cannot ping my Duckiedrone with hostname, but I can with its IP address.
---
`mDNS` is unavailable on your network or is being filtered. Try a phone hotspot named `duckietown` with password `quackquack`, then reboot the Duckiedrone to isolate the network issue. If it works, ask the original network's administrator to allow mDNS on the relevant subnet.
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
