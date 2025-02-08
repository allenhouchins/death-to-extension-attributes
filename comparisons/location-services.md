# Location Services

This Extension Attribute displays whether or not location services are enabled.
 
## Extension Attribute:
```
#!/bin/bash

uuid=$(system_profiler SPHardwareDataType | awk -F ": " '/Hardware UUID/ {print $2}')

domain="/var/db/locationd/Library/Preferences/ByHost/com.apple.locationd.${uuid}"
plist="${domain}.plist"

if [[ -f "$plist" ]]; then
  status=$(/usr/bin/defaults read "$domain" LocationServicesEnabled 2>/dev/null)

  if [[ "$status" == "1" ]]; then
    result="Enabled"
  else
    result="Disabled"
  fi
else
  result="Unavailable"
fi

echo "<result>${result}</result>"
```
## Fleet query:
```SELECT enabled from location_services```

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS