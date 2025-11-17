# Account Type

The purpose of this Extension Attribute is to return whether the logged-in account is a domain user or a local user.

## Extension Attribute:
```
#!/bin/sh

########## variable-ing ##########

loggedInUser=$(/usr/bin/stat -f%Su "/dev/console")

########## main process ##########

# Check OriginalNodeName attribute to determine domain user status.
if /usr/bin/dscl . -read "/Users/$loggedInUser" OriginalNodeName 2>&1 | /usr/bin/grep -q "No such key"; then
  accountType="Local User"
else
  accountType="Domain User"
fi

# Report result.
echo "<result>$accountType</result>"

exit 0
```
## Fleet query:
```SELECT uid, username, CASE WHEN directory NOT LIKE '/Users/%' THEN 'Domain User' ELSE 'Local User' END AS account_type FROM users WHERE uid >= 500 AND uid < 2147483647;```

Compatible with: ✅ macOS ✅ Windows ✅ Linux 🚫 ChromeOS

