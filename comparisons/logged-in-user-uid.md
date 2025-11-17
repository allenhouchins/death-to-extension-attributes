# Logged-In User UID

The purpose of this Extension Attribute is to get the UID of the currently logged-in user.

## Extension Attribute:
```
#!/bin/sh

########## variable-ing ##########

loggedInUser=$(/usr/bin/stat -f%Su "/dev/console")
loggedInUID=$(/usr/bin/id -u "$loggedInUser")

########## main process ##########

# Report UID of logged-in user.
echo "<result>$loggedInUID</result>"

exit 0
```
## Fleet query:
```SELECT uid, username FROM logged_in_users;```

Compatible with: ✅ macOS ✅ Windows ✅ Linux 🚫 ChromeOS

