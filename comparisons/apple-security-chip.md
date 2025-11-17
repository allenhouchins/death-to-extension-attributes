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
```SELECT json_extract(value, '$.model_name') AS security_chip FROM system_profiler WHERE data_type = 'SPiBridgeDataType';```

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS

