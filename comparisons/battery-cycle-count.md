# Battery Cycle Count

The purpose of this Extension Attribute is to return the cycle count of battery for notebooks (returns 0 for desktops).

## Extension Attribute:
```
#!/bin/sh

########## variable-ing ##########

powerReport=$(/usr/sbin/system_profiler SPPowerDataType)

########## main process ##########

# Count cycles (if computer has a battery).
if echo "$powerReport" | /usr/bin/grep -q "Battery Information"; then
  cycleCount=$(echo "$powerReport" | /usr/bin/awk '/Cycle Count/ {print $NF}' | /usr/bin/bc)
else
  cycleCount=0
fi

# Report result.
echo "<result>$cycleCount</result>"

exit 0
```
## Fleet query:
```SELECT cycle_count FROM battery;```

Compatible with: ✅ macOS ✅ Windows 🚫 Linux 🚫 ChromeOS

