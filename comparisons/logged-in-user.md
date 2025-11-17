# Logged-In User

The purpose of this Extension Attribute is to return the currently logged-in user.

## Extension Attribute:
```
#!/bin/sh

########## variable-ing ##########

loggedInUser=$(/usr/bin/stat -f%Su "/dev/console")

########## main process ##########

# Report logged-in user.
echo "<result>$loggedInUser</result>"

exit 0
```
## Fleet query:
```SELECT uid, username FROM logged_in_users;```

Compatible with: ✅ macOS ✅ Windows ✅ Linux 🚫 ChromeOS

