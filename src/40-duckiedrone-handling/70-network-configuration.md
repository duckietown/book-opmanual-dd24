```{seo}
:description: Step by step instructions on how to manage networks on an ente Duckiedrone.
:keywords: Duckiedrone, DD24, DD24-B, network management, network, Wi-Fi, wifi
```

```{needget}
- A flashed microSD card: [](dd24-sw-init)
---
- Knowledge of how to manage networks on a Duckiedrone
```

(dd24-network-config)=
# Network management

The Duckiedrone network is managed through `netplan`. To add, remove, or edit Wi-Fi networks, access the running Duckiedrone through SSH or remove its microSD card and mount it on your base station.

Any of the following methods work:

1. SSH into your Duckiedrone if you are already on the same network.

2. Ethernet: connect your Duckiedrone's Raspberry Pi to your router with an Ethernet cable, then connect to it with SSH.

3. Use the base station directly: power down the Duckiedrone, remove its microSD card, and connect it to your base station. After modifying the network configuration files as described below, return the card to the Duckiedrone before powering it on.

4. Create a network with SSID `duckietown` and password `quackquack`, e.g., with a smartphone in hotspot mode. Reboot the Duckiedrone and it will connect to it. Connect your base station to the same network, then connect to the Duckiedrone with SSH.

5. Reflash the microSD card: use this only as a last resort. If all else fails, reflash your microSD card using [the advanced way](dd24-sw-init-adv) and specify your network credentials.

To SSH into your Duckiedrone:

```bash
ssh duckie@ROBOT_NAME.local
```

The default password is `quackquack`.

To inspect the current Netplan configuration:

```bash
sudo netplan get
```

The command prints the merged Netplan configuration. For a Duckiedrone configured with one Wi-Fi network, the output resembles:

```yaml
network:
  version: 2
  wifis:
    wlan0:
      dhcp4: true
      optional: true
      access-points:
        "my-network":
          password: "<redacted>"
```

`netplan get` can print Wi-Fi names and passwords. Redact them before sharing its output. If it warns that the permissions for a file in `/etc/netplan` are too open, restrict the named runtime configuration file:

```bash
sudo chmod 600 /etc/netplan/<file>.yaml
```

(dd24-network-config-edit)=
## How to add, remove or edit networks

Choose **Persistent changes** to retain network additions after a reboot, or **Non-persistent changes** to use them only until the next reboot.

::::{tab-set}

:::{tab-item} Persistent changes

Once you have gained access, navigate to

```bash
cd /config/wifi
```

`/config/wifi` stores the persistent Wi-Fi configuration. Changes made here take effect after the Duckiedrone reboots.

There are three main files there:

- `00-user.yaml`: edit this file to manage new networks.

- `98-auto.yaml`: edit this file to modify network credentials provided during the `dts init_sd_card` procedure.

- `99-duckietown.yaml`: contains default backup `duckietown` network credentials and is best left untouched.

```{note}
`00-user.yaml` is reserved for user-managed Wi-Fi networks. Add new networks here and avoid modifying `98-auto.yaml` or `99-duckietown.yaml` unless you intentionally want to change the networks configured by `dts init_sd_card` or the default Duckietown access point.
```

Navigate to `/config/wifi/00-user.yaml` and add new network credentials following the structure detailed in [](dd24-network-config-info).

```bash
sudo nano /config/wifi/00-user.yaml
```

You will see:

```yaml
network:
  version: 2
  wifis:
    wlan0:
      dhcp4: true
      optional: true
      access-points:
        # Add your Wi-Fi networks here. This is an example, modify it to match your network configuration
        "my-ssid":
          password: "my-password"
```

Replace `my-ssid` and `my-password` with the credentials of a new network. To add multiple networks, follow this structure:

```yaml
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

        "duckietown":
          password: "quackquack"
```

Note that:

- Indentation matters (2 spaces throughout, no tabs).

- The Raspberry Pi will automatically connect to any known network that is in range. You do not need to specify a priority in the common case.

- If several known networks are in range, the active networking backend selects one according to its configuration and the networks' availability.

Save and exit `nano`, then reboot the Duckiedrone:

```bash
sudo reboot
```

```{warning}
Restarting with a new Wi-Fi configuration may connect the Duckiedrone to a different known network. If you are connected over SSH, your session will be interrupted. Ensure your computer is connected to the same Wi-Fi network as the Duckiedrone before reconnecting.
```

:::

:::{tab-item} Non-persistent changes

Once you have gained access, navigate to

```bash
cd /etc/netplan
```

`/etc/netplan` holds the runtime Netplan configuration. Changes made here are discarded on the next reboot.

There are three main files there:

- `00-user.yaml`: edit this file to manage new networks.

- `98-auto.yaml`: edit this file to modify network credentials provided during the `dts init_sd_card` procedure.

- `99-duckietown.yaml`: contains default backup `duckietown` network credentials and is best left untouched.

```{note}
`00-user.yaml` is reserved for user-managed Wi-Fi networks. Add new networks here and avoid modifying `98-auto.yaml` or `99-duckietown.yaml` unless you intentionally want to change the networks configured by `dts init_sd_card` or the default Duckietown access point.
```

Navigate to `/etc/netplan/00-user.yaml` and add new network credentials following the structure detailed in [](dd24-network-config-info).

```bash
sudo nano /etc/netplan/00-user.yaml
```

You will see:

```yaml
network:
  version: 2
  wifis:
    wlan0:
      dhcp4: true
      optional: true
      access-points:
        # Add your Wi-Fi networks here. This is an example, modify it to match your network configuration
        "my-ssid":
          password: "my-password"
```

Replace `my-ssid` and `my-password` with the credentials of a new network. To add multiple networks, follow this structure:

```yaml
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

        "duckietown":
          password: "quackquack"
```

Note that:

- Indentation matters (2 spaces throughout, no tabs).

- The Raspberry Pi will automatically connect to any known network that is in range. You do not need to specify a priority in the common case.

- If several known networks are in range, the active networking backend selects one according to its configuration and the networks' availability.

Save and exit `nano`, then test the new configuration:

```bash
sudo netplan try
```

```{warning}
`netplan try` waits for confirmation and rolls back automatically if you do not confirm it. It may interrupt an SSH session if the Duckiedrone connects to a different network. Ensure your computer can reach the replacement network before confirming the change.
```

:::
::::

(dd24-network-config-info)=
### Additional network settings

If you add Wi-Fi networks manually, use the example that matches your network's authentication:

- Unprotected (Open) Wi-Fi network:

```yaml
access-points:
  "<ssid>": {}
```

- WPA Personal Wi-Fi network with PSK authentication:

```yaml
access-points:
  "<ssid>":
    auth:
      key-management: psk
      password: "<wifi_password>"
```

- WPA Enterprise Wi-Fi network with PEAP authentication:

```yaml
access-points:
  "<ssid>":
    auth:
      key-management: eap
      identity: "<username>"
      password: "<password>"
      method: peap
      phase2-auth: mschapv2
```

- WPA Enterprise Wi-Fi network with EAP-TLS authentication:

```yaml
network:
  version: 2
  wifis:
    wlan0:
      dhcp4: true
      access-points:
        "<ssid>":
          auth:
            key-management: eap
            method: tls
            identity: "<username>"
            ca-certificate: "/etc/ssl/certs/ca-cert.pem"
            client-certificate: "/etc/ssl/certs/client-cert.pem"
            client-key: "/etc/ssl/private/client-key.pem"
            client-key-password: "client-key-passphrase"
```

- Advanced client options (static IP and Wi-Fi frequency settings):

```yaml
network:
  version: 2
  wifis:
    wlan0:
      dhcp4: false
      addresses: [192.168.1.50/24]
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses: [192.168.1.1]  # Replace with your DNS server
      access-points:
        "Your_SSID_Name":
          password: "Your_Password"
          bssid: "AA:BB:CC:DD:EE:FF"  # Forces connection to this exact router
          band: 5GHz                 # Restricts band to 5GHz (Options: 5GHz or 2.4GHz)
          channel: 36                # Optional specific channel
```
