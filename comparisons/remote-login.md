# Remote Login

This Extension Attribute displays whether or not remote login enabled.
 
## Extension Attribute:
```
#!/bin/bash

OS=$(sw_vers -productVersion | awk -F. '{print $2}')

# Check if macOS version is older than 10.5
if [[ "$OS" -lt 5 ]]; then
    # Check if the older systemsetup binary exists
    if [[ -f /System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Support/systemsetup ]]; then
        result=$(/System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Support/systemsetup -getremotelogin | awk '{print $3}')
    else
        result="The systemsetup binary is not present on this machine."
    fi
else
    # Use modern systemsetup binary
    result=$(/usr/sbin/systemsetup -getremotelogin | awk '{print $3}')
fi


echo "<result>${result}</result>"
```
## Fleet query:
```SELECT * FROM sharing_preferences WHERE remote_login='1'```

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS