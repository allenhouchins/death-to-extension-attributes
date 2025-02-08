# File Sharing

This Extension Attribute displays whether or not file sharing is enabled.
 
## Extension Attribute:
```
#!/bin/sh

OS=$(/usr/bin/sw_vers -productVersion | /usr/bin/colrm 5)

if [[ "$OS" < "10.5" ]]; then
    # Check AFP (Apple File Protocol) status from /private/etc/hostconfig
    result=$(grep AFP /private/etc/hostconfig | sed 's/AFPSERVER=//g')

    if [ "$result" = "-Yes-" ]; then
        echo "<result>On</result>"
    elif [ "$result" = "-No-" ]; then
        echo "<result>Off</result>"
    else
        echo "<result>Unknown</result>"
    fi
else
    # Check if AppleFileServer is running
    result=$(/bin/launchctl list | grep "AppleFileServer")

    if [ -z "$result" ]; then
        echo "<result>Off</result>"
    else
        echo "<result>On</result>"
    fi
fi
```
## Fleet query:
```SELECT * FROM sharing_preferences WHERE file_sharing='1'```

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS