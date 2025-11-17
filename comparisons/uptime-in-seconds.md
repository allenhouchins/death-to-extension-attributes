# Uptime in Seconds

The purpose of this Extension Attribute is to return the system uptime in seconds.

## Extension Attribute:
```
#!/bin/sh

########## variable-ing ##########

currentDate=$(/bin/date +%s)
bootDate=$(/usr/sbin/sysctl -n kern.boottime | /usr/bin/awk -F'[ ,]' '{print $4}')

########## main process ##########

# Get current uptime by subtracting last boot date from current date in seconds.
uptime=$(( currentDate - bootDate ))

# Report result.
echo "<result>$uptime</result>"

exit 0
```
## Fleet query:
```SELECT total_seconds FROM uptime;```

Compatible with: ✅ macOS ✅ Windows ✅ Linux 🚫 ChromeOS

