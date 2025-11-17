# Battery Charge

The purpose of this Extension Attribute is to report current battery charge as a percentage.

## Extension Attribute:
```
#!/bin/sh

########## main process ##########

# Get battery charge percentage.
batteryChargePercentage=$(/usr/bin/pmset -g batt | /usr/bin/awk '/%/ {print $3}' | /usr/bin/tr -d '%;' | /usr/bin/bc)

# Report results.
echo "<result>${batteryChargePercentage}</result>"

exit 0
```
## Fleet query:
```SELECT battery_level, is_charging FROM battery;```

Compatible with: ✅ macOS ✅ Windows 🚫 Linux 🚫 ChromeOS

