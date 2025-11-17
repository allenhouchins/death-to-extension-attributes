# Startup Volume

The purpose of this Extension Attribute is to report the name of the startup volume.

## Extension Attribute:
```
#!/bin/sh

########## main process ##########

echo "<result>$(/usr/sbin/diskutil info -plist "$(bless --getBoot)" | /usr/bin/plutil -extract VolumeName raw -- -)</result>"

exit 0
```
## Fleet query:
```SELECT device, path, label FROM mounts WHERE path = '/';```

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS

