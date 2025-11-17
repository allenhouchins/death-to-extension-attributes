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
```SELECT key, value, CASE WHEN value = '1' THEN 'Enabled' ELSE 'Disabled' END AS location_services_status FROM plist WHERE path LIKE '/var/db/locationd/Library/Preferences/ByHost/com.apple.locationd.%' AND key = 'LocationServicesEnabled';```

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS