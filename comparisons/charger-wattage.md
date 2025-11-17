# Charger Wattage

The purpose of this Extension Attribute is to report the wattage of the power adapter (if connected).

## Extension Attribute:
```
#!/bin/sh

########## main process ##########

# Report results.
echo "<result>$(/usr/sbin/system_profiler SPPowerDataType | /usr/bin/awk '/Wattage/ {print $NF}')</result>"

exit 0
```
## Fleet query:
```SELECT battery_level, is_charging, CASE WHEN is_charging = 1 THEN 'Connected' ELSE 'Not Connected' END AS charger_status FROM battery;```

**Note:** osquery doesn't directly expose charger wattage, but can detect charging status. For actual wattage, you may need a custom table or system_profiler command.

Compatible with: ✅ macOS ✅ Windows 🚫 Linux 🚫 ChromeOS

