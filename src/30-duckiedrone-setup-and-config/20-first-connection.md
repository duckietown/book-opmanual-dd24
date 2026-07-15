```{seo}
:description: Learn how to connect to your Duckiedrone (DD24) using the Duckietown Dashboard in client or access point mode.
:keywords: Duckiedrone first connection, DD24 network setup, Duckietown Dashboard, AP mode, CL mode, robotics Wi-Fi configuration
```

```{needget}

* A live Duckiedrone: [](sec:first-boot)
* A properly configured network: [](dd24-network-config)
* A properly configured base station: [](dd24-initial-setup)  
---

*   A connected Duckiedrone
```

(first_connection)=
# First connection

You are now ready to connect to your Duckiedrone.

(dd24-how-to-connect)=
## Connecting to the Duckiedrone  

Connection the base station and the Duckiedrone is an essential, and often non-trivial, step.

The first thing to test is if your computer and Duckidrone are able to see communicate over the network. On your base station, open a terminal and:

    ping [hostname].local

````{admonition} A succesfull ping example
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

If `ping` does not work nothing else will. 





    - You are able to access the robot's Dashboard at `http://[hostname].local`, or through the command `dts duckiebot dashboard [hostname]`
    - You are able to `ssh` into the Duckiedrone with `ssh duckie@[hostname].local`

    If any of these basic interfacing commands are not working, the most likely cause is that either your computer and robot are not on the same network, or the first boot procedure is not complete yet. 





Make sure you are on the same network as your Duckiedrone:
    
::::{tab-set}

:::{tab-item} Client (CL) mode

Connect to the same network that the drone is connected to if the drone is in CL mode.

The default network is `duckietown` (password: `quackquack`)  

:::

:::{tab-item} Access Point (AP) mode

Connect to `duckietown-<hostname>-ap` if the drone is in AP mode, where `<hostname>` is the robot name chosen during the initialization procedure.

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

*   SSID: `duckietown-amelia-ap`
    
*   Password: `quackquack`
    

**Default** client (**CL**) network configuration:

*   SSID: `duckietown`
    
*   Password: `quackquack`
```

## Troubleshooting

```{trouble}
I cannot connect to my Duckiedrone in AP mode.
---
Try using client mode and shut down the Docker container `dt-access-point` through the Portainer interface (accessible through your browser from your base station at `<hostname>.local:9000`).
```
