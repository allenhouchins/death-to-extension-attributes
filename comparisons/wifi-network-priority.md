# Wi-Fi Network Priority

The purpose of this Extension Attribute is to list all preferred Wi-Fi networks in order of priority.

## Extension Attribute:
```
#!/bin/sh

########## variable-ing ##########

preferredWiFiNetworks="$(/usr/sbin/networksetup -listpreferredwirelessnetworks en0 | /usr/bin/sed 1d | /usr/bin/sed 's/^	*//g')"

########## main process ##########

echo "<result>${preferredWiFiNetworks}</result>"

exit 0
```
## Fleet query:
```SELECT interface, ssid, bssid FROM wifi_networks WHERE interface = 'en0' ORDER BY ssid;```

**Note:** osquery doesn't directly expose network priority order. This query lists available networks but may not reflect the exact priority order from system preferences.

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS

