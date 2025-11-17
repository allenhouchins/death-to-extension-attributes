# Hostname

The purpose of this Extension Attribute is to return the hostname of the computer.

## Extension Attribute:
```
#!/bin/sh

########## main process ##########

# Report hostname.
echo "<result>$(/bin/hostname 2>&1)</result>"

exit 0
```
## Fleet query:
```SELECT hostname, computer_name FROM system_info;```

Compatible with: ✅ macOS ✅ Windows ✅ Linux 🚫 ChromeOS

