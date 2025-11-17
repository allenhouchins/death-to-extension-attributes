# Wi-Fi Network

The purpose of this Extension Attribute is to display the current Wi-Fi network for the default network adapter (en0).

## Extension Attribute:
```
#!/bin/sh

########## main process ##########

# Report results.
echo "<result>$(/usr/sbin/networksetup -getairportnetwork en0 2>"/dev/null" | /usr/bin/awk -F 'Current Wi-Fi Network: ' '{print $2}')</result>"

exit 0
```
## Fleet query:
```SELECT interface, ssid, bssid FROM wifi_networks WHERE interface = 'en0';```

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS

