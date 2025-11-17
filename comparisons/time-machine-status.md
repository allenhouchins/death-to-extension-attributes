# Time Machine Status

The purpose of this Extension Attribute is to return whether Time Machine is enabled.

## Extension Attribute:
```
#!/bin/sh

########## variable-ing ##########

timeMachineAutoBackup=$(/usr/bin/defaults read "/Library/Preferences/com.apple.TimeMachine.plist" AutoBackup 2>"/dev/null")

########## main process ##########

# Get Time Machine AutoBackup setting.
if [ "$timeMachineAutoBackup" = "1" ]; then
  timeMachineStatus="Enabled"
else
  timeMachineStatus=""
fi

# Report result.
echo "<result>$timeMachineStatus</result>"

exit 0
```
## Fleet query:
```SELECT key, value FROM plist WHERE path = '/Library/Preferences/com.apple.TimeMachine.plist' AND key = 'AutoBackup';```

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS

