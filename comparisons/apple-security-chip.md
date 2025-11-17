# Apple Security Chip

The purpose of this Extension Attribute is to report which generation of Apple Security Chip is present (if any).

## Extension Attribute:
```
#!/bin/sh

########## main process ##########

echo "<result>$(/usr/sbin/system_profiler SPiBridgeDataType | /usr/bin/awk -F ': ' '/Model Name/ {print $NF}')</result>"

exit 0
```
## Fleet query:
```SELECT hardware_model, hardware_serial FROM system_info;```

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS

