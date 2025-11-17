# Osquery Version

The purpose of this Extension Attribute is to return Osquery version if installed.

## Extension Attribute:
```
#!/bin/sh

########## variable-ing ##########

osquerydPath="/opt/osquery/lib/osquery.app/Contents/MacOS/osqueryd"
osquerydLegacyPath="/usr/local/bin/osqueryd"
osqueryVersion=""

########## main process ##########

# Check for presence of target binary and get version.
if [ -e "$osquerydPath" ]; then
  osqueryVersion=$("$osquerydPath" --version | /usr/bin/awk '{print $3}')
# Legacy binary path for Osquery < 5.x.
elif [ -e "$osquerydLegacyPath" ]; then
  osqueryVersion=$("$osquerydLegacyPath" --version | /usr/bin/awk '{print $3}')
fi

# Report result.
echo "<result>${osqueryVersion}</result>"

exit 0
```
## Fleet query:
```SELECT version FROM osquery_info;```

Compatible with: ✅ macOS ✅ Windows ✅ Linux 🚫 ChromeOS

