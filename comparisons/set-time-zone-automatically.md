# Set Time Zone Automatically

The purpose of this Extension Attribute is to read whether "set time zone automatically using current location setting" is enabled.

## Extension Attribute:
```
#!/bin/sh

########## variable-ing ##########

defaultsRead=$(/usr/bin/defaults read "/private/var/db/timed/Library/Preferences/com.apple.timed" TMAutomaticTimeZoneEnabled 2>"/dev/null")

########## main process ##########

# Parse output of defaults write.
if [ "$defaultsRead" = "1" ]; then
  setTimeZoneAutomatically="enabled"
else
  setTimeZoneAutomatically="disabled"
fi

# Report results.
echo "<result>${setTimeZoneAutomatically}</result>"

exit 0
```
## Fleet query:
```SELECT key, value, CASE WHEN value = '1' THEN 'enabled' ELSE 'disabled' END AS time_zone_auto_setting FROM plist WHERE path = '/private/var/db/timed/Library/Preferences/com.apple.timed' AND key = 'TMAutomaticTimeZoneEnabled';```

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS

