```{seo}
:description: Learn how to keep your Duckiedrone software up to date using the Duckietown Shell.
:keywords: Duckiedrone first connection, DD24 network setup, Duckietown Dashboard, AP mode, CL mode, robotics Wi-Fi configuration
```

```{needget}

* A connected Duckiedrone: [](dd24-how-to-connect)
* A functional Duckietown Shell (`dts`) installation: [](d24-required-sw-and-accounts)
* up to 15 minutes
---

* An up to date Duckiedrone
```

(dd24-software-update)=
# Software update

To keep the software running on your Duckiedrone up to date, on your base station run:

    dts duckiebot update ROBOTNAME

where `ROBOTNAME` is your Duckiedrone's hostname. 


```{attention}
Always perform a Duckiedrone software update after the first boot and connection.
```

<!--
## Troubleshooting

If any of these basic interfacing commands are not working, the most likely causes are:

- either your computer or robot are not on the same network, 
- the Duckiedrone's [first boot procedure](sec:first-boot) is not complete yet
- the network you are trying to use does not have `mDNS` active.

A general alternative networking solution that bypasses Wi-Fi, and can be useful during debugging, is connecting the Duckiedrone via Ethernet cable to the router, if you have physical access to it.

```{trouble}
I cannot ping my Duckiedrone with hostname, but I can with its IP address. 
---
`mdNS` is not activated on your network. Try a simple `duckietown:quackquack` hotspot with your phone and reboot the Duckiedrone to confirm. 
```

## Other notes on Duckiedrone networking (AP)

The Duckiedrone experimentally supports access point (AP) network configuration too, a setup in which it is the robot itself emitting the network, and the base station connecting to it. This feature is currently disabled until further testing is conducted. These instructions will be updated in due time to document this feature. 


```{trouble}
I cannot connect to my Duckiedrone in AP mode.
---
Try using client mode and shut down the Docker container `dt-access-point` through the Portainer interface (accessible through your browser from your base station at `<hostname>.local:9000`).
```


-->