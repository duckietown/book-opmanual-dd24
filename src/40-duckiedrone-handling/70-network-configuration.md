```{seo}
:description: Step by step instructions on how to manage networks on an ente Duckiedrone.
:keywords: Duckiedrone, DD24, DD24-B, network management, network, Wi-Fi, wifi
```

```{needget}
* A flashed SD card: [](dd24-sw-init)
---
* Knowledge on how to manage networks on a Duckiedrone
```

(dd24-network-config)=
# Network configuration

The Duckiedrone network is managed through `netplan`. To add, remove or edit Wi-Fi networks, you will need to establish a way to connect to your Duckiedrone's SD card. Any of the following methods work:

1. SSH: into your Duckiedrone, if you are already on the same network.
2. Ethernet: connect your Duckiedrone's Raspberry Pi to your router via ethernet cable, then ssh into it.
3. Use directly the base station: remove the SD card from the Duckiedrone, and connect it to your base station. You will then be able to access to the SD card's files. After modifying the network configuration files as described below, place the card back in the Duckiedrone.
4. Create a network with ssid `duckietown` and password `quackquack`, e.g., with a smartphone in hotspot mode. Reboot the drone and it will connect to it. Connect your base station to the same network and ssh into the robot. 
5. Reflash the SD card: should be used only as last resort, but if all else fails, reflash your SD card using [the advanced way](dd24-sw-init-adv), and specify your networ's credentials.  

To ssh into your Duckiebot: 

    ssh duckie@robotname.local
   
the default password is `quackquack`. 

To check which networks are currently defined:

    sudo wpa_cli list_networks

Once gained access, navigate to

    cd /etc/netplan

There are three main files there: 

* `00-user.yaml`: edit this file to manage new networks.
* `98-auto.yaml`: edit this file to modify network credentials provided during the `dts init_sd_card` procedure.
* `99-duckietown.yaml`: contains default backup `duckietown` network credentials - best if not touched.

```{note}
`00-user.yaml` is reserved for user-managed Wi-Fi networks. Add new networks here and avoid modifying `98-auto.yaml` or `99-duckietown.yaml` unless you intentionally want to change the networks configured by `dts init_sd_card` or the default Duckietown access point.
```

(dd24-network-config-edit)=
## How to add, remove or edit networks

Navigate to `/etc/netplan/00-user.yaml` and add new network credentials following the structure detailed in [](dd24-network-config-info).

    sudo nano /etc/netplan/00-user.yaml

You will see:

    network:
        version: 2
        wifis:
            wlan0:
            dhcp4: true
            optional: true
            access-points:
                # Add your WiFi networks here. This is an example, modify it to match your network configuration
                "my-ssid":
                password: "my-password"

Replace `my-ssid` and `my-password` with the credentials of a new network. To add multiple networks, follow this structure: 

```shell
network:
  version: 2
  wifis:
    wlan0:
      dhcp4: true
      optional: true
      access-points:
        "Home WiFi":
          password: "home-password"

        "University":
          password: "campus-password"

        "Phone Hotspot":
          password: "hotspot-password"

        "Duckietown":
          password: "quackquack"
```

Note that:

* Indentation matters (2 spaces throughout, no tabs).
* The Raspberry Pi will automatically connect to any known network that is in range. You do not need to specify a priority in the common case.
* If two known networks are available simultaneously, `wpa_supplicant` will choose according to its own selection logic (signal quality, security, etc.).

Save and exit `nano`, then: 

    sudo netplan apply
    sudo reboot

```{warning}
Applying a new Wi-Fi configuration may cause the Duckiedrone to immediately connect to a different known network. If you are connected over SSH, your session will be interrupted. Ensure your computer is connected to the same Wi-Fi network as the Duckiedrone before reconnecting.
```

(dd24-network-config-info)=
### Additional optional network settings parameters

If you add Wi-Fi networks manually, follow the example below relative to the network authentication you have:


 - Unprotected (Open) WiFi network:

```shell
   ...
      access-points:
        "<ssid>": {{}}
```


 - WPA/WPA2 WiFi network with PSK authentication:

```shell
   ...
      access-points:
        "<ssid>":
          key-management: psk
          password: "<wifi_password>"
```


 - WPA/WPA2 WiFi network with username/password authentication:

```shell
   ...
      access-points:
        "<ssid>":
          auth:
            key-management: eap
            identity: "<username>"
            password: "<password>"
            method: peap
            phase2-auth: MSCHAPV2
```

- WPA3 (SAE) & Enterprise (EAP-TLS) Authentication

```shell
network:
  version: 2
  renderer: NetworkManager
  wifis:
    wlan0:
      dhcp4: yes
      access-points:
        "WPA3_Enterprise_Network":
          auth:
            key-management: eap
            method: tls
            identity: "username@yourdomain.com"
            ca-certificate: "/etc/ssl/certs/ca-cert.pem"
            client-certificate: "/etc/ssl/certs/client-cert.pem"
            client-key: "/etc/ssl/private/client-key.pem"
            client-key-password: "client-key-passphrase"
```

- Advanced Client options (Static IP, Frequency settings)

```shell
network:
  version: 2
  renderer: NetworkManager
  wifis:
    wlan0:
      dhcp4: no
      addresses: [192.168.1.50/24]
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses:
      access-points:
        "Your_SSID_Name":
          password: "Your_Password"
          bssid: "AA:BB:CC:DD:EE:FF"  # Forces connection to this exact router
          band: 5GHz                 # Restricts band to 5GHz (Options: 5GHz or 2.4GHz)
          channel: 36                # Optional specific channel
```