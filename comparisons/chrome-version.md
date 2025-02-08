# Chrome Version

This Extension Attribute displays version of Chrome installed.
 
## Extension Attribute:
```
#!/bin/sh

CHROME_APP="/Applications/Google Chrome.app"
INFO_PLIST="$CHROME_APP/Contents/Info.plist"

if [ -d "$CHROME_APP" ]; then
    RESULT=$(/usr/bin/defaults read "$INFO_PLIST" CFBundleShortVersionString 2>/dev/null)
    echo "<result>${RESULT:-Unknown}</result>"
else
    echo "<result>Not Installed</result>"
fi

```
## Fleet query:
```SELECT bundle_short_version FROM apps WHERE name = 'Google Chrome.app'```

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS