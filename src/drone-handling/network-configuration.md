```{seo}
:description: Configure Wi-Fi networks on your Duckiedrone (DD24), including corporate networks like eduroam, for reliable connectivity.
:keywords: Duckiedrone network configuration, DD24 WiFi setup, wpa_supplicant configuration, eduroam setup, enterprise networks, WPA-EAP, drone networking
```

(dd24-network-configuration)=
# Network Configuration

This page describes how to configure Wi-Fi networks on your Duckiedrone, including enterprise networks such as eduroam.

```{needget}
* A Duckiedrone with SD card access
* Basic understanding of network settings
---
* A Duckiedrone connected to your desired networks
```

## Overview

Your Duckiedrone uses the `wpa_supplicant` service to manage Wi-Fi connections. Network configurations are stored in the `wpa_supplicant.conf` file, which can be edited to add new networks or modify existing ones.

```{attention}
Changes to network configuration require either editing the SD card directly or accessing the Duckiedrone via SSH. Make sure to back up your configuration before making changes.
```

## Adding Wi-Fi Networks

### Method 1: Edit SD Card Before First Boot

If you have not booted your Duckiedrone yet, you can configure networks during the [software initialization](sw-initialization) process:

1. After flashing the SD card, navigate to the `config` partition
2. Open the `wpa_supplicant.conf` file with a text editor
3. Add your network configurations as described below

### Method 2: Edit Configuration on Running Duckiedrone

For an already initialized Duckiedrone, you can modify network settings by:

1. SSH into your Duckiedrone:
   ```bash
   ssh duckie@[ROBOT_NAME].local
   ```

2. Edit the configuration file:
   ```bash
   sudo nano /config/wpa_supplicant.conf
   ```

3. Add your network configurations and save the file

4. Restart the robot:
   ```bash
   sudo reboot
   ```

(dd24-network-configuration-examples)=
## Network Configuration Examples

### Basic Home/Office Network (WPA-PSK)

For most home and office networks with WPA/WPA2 security:

```bash
network={
    id_str="home_network"
    ssid="MyHomeWiFi"
    psk="my_wifi_password"
    key_mgmt=WPA-PSK
    priority=5
}
```

### Open Network (No Password)

For open networks without passwords:

```bash
network={
    id_str="open_network"
    ssid="FreeWiFi"
    key_mgmt=NONE
    priority=1
}
```

### Hidden Network

For networks that do not broadcast their SSID:

```bash
network={
    id_str="hidden_network"
    ssid="HiddenNetwork"
    psk="hidden_password"
    key_mgmt=WPA-PSK
    scan_ssid=1
    priority=3
}
```

## Enterprise Networks (WPA-EAP)

### eduroam Configuration

For eduroam networks commonly found at universities and research institutions:

```bash
network={
    ssid="eduroam"
    key_mgmt=WPA-EAP
    eap=PEAP
    identity="your_username@yourdomain.edu"
    password="your_password"
    phase1="peaplabel=0"
    phase2="auth=MSCHAPV2"
    ca_cert="/etc/ssl/certs/ca-certificates.crt"
    priority=10
}
```

**Parameter explanation:**
* `ssid="eduroam"`: The SSID of the network
* `key_mgmt=WPA-EAP`: Enterprise WPA authentication
* `eap=PEAP`: Most institutions use PEAP for eduroam
* `identity`: Your login credentials (e.g., `user@university.edu`)
* `password`: Your password ⚠️ **stored in plain text** unless you use `wpa_passphrase`
* `phase1` and `phase2`: Required for PEAP/MSCHAPv2
* `ca_cert`: Path to CA certificates. `/etc/ssl/certs/ca-certificates.crt` is standard for Ubuntu
* `priority`: Helps when multiple networks are available (higher number = higher priority)

```{warning}
**Security Notice**: Passwords in `wpa_supplicant.conf` are stored in plain text. For better security, consider using certificate-based authentication when possible.
```

### Enterprise Network with Custom Certificate

If your institution provides a custom CA certificate:

```bash
network={
    ssid="enterprise_network"
    key_mgmt=WPA-EAP
    eap=PEAP
    identity="username@company.com"
    password="your_password"
    phase1="peaplabel=0"
    phase2="auth=MSCHAPV2"
    ca_cert="/path/to/custom-ca.crt"
    priority=8
}
```

### Enterprise Network with TTLS

For networks using TTLS instead of PEAP:

```bash
network={
    ssid="corporate_wifi"
    key_mgmt=WPA-EAP
    eap=TTLS
    identity="domain\\username"
    password="your_password"
    phase2="auth=PAP"
    ca_cert="/etc/ssl/certs/ca-certificates.crt"
    priority=7
}
```

## Complete Configuration File Example

Here's a complete `wpa_supplicant.conf` file with multiple network configurations:

```bash
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US

# Default Duckietown network
network={
    id_str="duckietown_default"
    ssid="duckietown"
    psk="quackquack"
    key_mgmt=WPA-PSK
    priority=5
}

# Home network
network={
    id_str="home"
    ssid="MyHomeWiFi"
    psk="home_password"
    key_mgmt=WPA-PSK
    priority=6
}

# University eduroam
network={
    ssid="eduroam"
    key_mgmt=WPA-EAP
    eap=PEAP
    identity="student@university.edu"
    password="student_password"
    phase1="peaplabel=0"
    phase2="auth=MSCHAPV2"
    ca_cert="/etc/ssl/certs/ca-certificates.crt"
    priority=10
}

# Office network
network={
    id_str="office"
    ssid="OfficeWiFi"
    psk="office_password"
    key_mgmt=WPA-PSK
    priority=7
}
```

## Configuration Parameters

### Important Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `ssid` | Network name | `"MyWiFi"` |
| `psk` | Pre-shared key (password) | `"mypassword"` |
| `key_mgmt` | Security type | `WPA-PSK`, `WPA-EAP`, `NONE` |
| `priority` | Connection priority (higher = preferred) | `10` |
| `id_str` | Network identifier | `"home_network"` |

### Enterprise Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `eap` | EAP method | `PEAP`, `TTLS`, `TLS` |
| `identity` | Username | `"user@domain.edu"` |
| `password` | Password | `"userpassword"` |
| `phase1` | Phase 1 parameters | `"peaplabel=0"` |
| `phase2` | Phase 2 authentication | `"auth=MSCHAPV2"` |
| `ca_cert` | CA certificate path | `"/etc/ssl/certs/ca-certificates.crt"` |

## Testing Network Connectivity

After configuring your networks, test the connection:

1. **Check network status:**
   ```bash
   iwconfig
   ```

2. **Test internet connectivity:**
   ```bash
   ping -c 4 google.com
   ```

3. **Check which network is connected:**
   ```bash
   iwgetid
   ```

4. **View available networks:**
   ```bash
   sudo iwlist scan | grep ESSID
   ```

## Troubleshooting

### Common Issues

```{trouble}
My Duckiedrone won't connect to the network
---
1. Check that the SSID and password are correct
2. Verify that the `country` code matches your location
3. Check signal strength: `iwlist scan | grep -A5 "YourNetworkName"`
4. Review the system logs: `sudo journalctl -u wpa_supplicant`
```

```{trouble}
eduroam connection fails
---
1. Verify your credentials with your IT department
2. Check if your institution requires specific certificate settings
3. Try alternative EAP methods (TTLS instead of PEAP)
4. Some institutions require a domain prefix: `domain\username`
```

```{trouble}
Network connects, but no internet access
---
1. Check DNS settings: `cat /etc/resolv.conf`
2. Test with different DNS: `nslookup google.com 8.8.8.8`
3. Some corporate networks require proxy settings
4. Check if your device needs to be registered on the network
```

### Advanced Debugging

For detailed debugging, enable verbose logging:

```bash
sudo wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf -d
```

View the logs in real-time:
```bash
sudo journalctl -f -u wpa_supplicant
```

## Security Best Practices

1. **Limit network priorities**: Set higher priorities only for trusted networks
2. **Use strong passwords**: Avoid common or easily guessable passwords
3. **Regular updates**: Keep your Duckiedrone software updated
4. **Monitor connections**: Regularly check which networks your drone connects to
5. **Certificate validation**: Always use proper CA certificates for enterprise networks

```{warning}
Be cautious when connecting to public networks. Consider using a VPN for additional security when operating on untrusted networks.
```

## Country-Specific Considerations

```{attention}
The `country` parameter in your configuration must match your physical location to comply with local radio frequency regulations. Using an incorrect country code may result in connection issues or violate local laws.
```

Common country codes:
- US: United States
- CA: Canada  
- GB: United Kingdom
- DE: Germany
- FR: France
- JP: Japan
- AU: Australia

See the [full list of country codes](https://www.arubanetworks.com/techdocs/InstantWenger_Mobile/Advanced/Content/Instant%20User%20Guide%20-%20volumes/Country_Codes_List.htm) for your specific location.

## Next Steps

Once your network configuration is complete:

1. Proceed to [First Connection](first_connection) to access your Duckiedrone
2. Check the [Troubleshooting](build_troubleshooting) guide if you encounter issues
3. Learn about [Flight Controller Initialization](../preliminaries/flight-controller-initialization.md) for advanced setup
