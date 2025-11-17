# Screen Time

The purpose of this Extension Attribute is to report whether Screen Time is enabled or disabled.

## Extension Attribute:
```
#!/bin/sh

########## variable-ing ##########

loggedInUser=$(/usr/bin/stat -f%Su "/dev/console")
loggedInUID=$(/usr/bin/id -u "$loggedInUser")
loggedInUserHome=$(/usr/bin/dscl . -read "/Users/${loggedInUser}" NFSHomeDirectory | /usr/bin/awk '{print $NF}')
plistPath="${loggedInUserHome}/Library/Containers/com.apple.ScreenTimeAgent/Data/Library/Preferences/com.apple.ScreenTimeAgent.plist"
plistKey="UsageGenesisDate"

########## function-ing ##########

# Exits if root is the currently logged-in user, or no logged-in user is detected.
check_logged_in_user () {
  if [ "$loggedInUser" = "root" ] || [ -z "$loggedInUser" ]; then
    echo "Nobody is logged in."
    exit 0
  fi
}

########## main process ##########

# Checks script prerequisites.
check_logged_in_user

# Read setting key and report enabled vs disabled.
currentKey=$(/bin/launchctl asuser "$loggedInUID" /usr/bin/defaults read "$plistPath" "$plistKey" 2>"/dev/null")
if [ -z "$currentKey" ]; then
  settingValue="disabled"
else
  settingValue="enabled"
fi

echo "<result>${settingValue}</result>"

exit 0
```
## Fleet query:
```SELECT key, value, CASE WHEN value IS NOT NULL AND value != '' THEN 'enabled' ELSE 'disabled' END AS screen_time_status FROM plist WHERE path LIKE '%/com.apple.ScreenTimeAgent.plist' AND key = 'UsageGenesisDate';```

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS

