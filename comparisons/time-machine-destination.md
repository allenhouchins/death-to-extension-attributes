# Time Machine Backup Location

This Extension Attribute displays the Time Machine backup destination.
 
## Extension Attribute:
```
#!/bin/sh

# Check if Time Machine AutoBackup is enabled
enabled=$(/usr/bin/defaults read /Library/Preferences/com.apple.TimeMachine AutoBackup 2>/dev/null)

if [ "$enabled" = "1" ]; then
    destinationUUID=$(/usr/bin/defaults read /Library/Preferences/com.apple.TimeMachine "DestinationVolumeUUID" 2>/dev/null)

    if [ -z "$destinationUUID" ]; then
        echo "<result>No backup destination found.</result>"
        exit 0
    fi

    validateDestination=$(/usr/sbin/diskutil info "$destinationUUID" 2>&1)

    if echo "$validateDestination" | grep -q "Could not"; then
        echo "<result>Destination not mounted</result>"
    else
        backupDestination=$(echo "$validateDestination" | grep "Mount Point" | awk -F': ' '{print $2}')
        echo "<result>$backupDestination</result>"
    fi
else
    echo "<result>Not enabled.</result>"
fi
```
## Fleet query:
```SELECT alias FROM time_machine_destinations```

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS