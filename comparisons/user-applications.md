# User Applications

The purpose of this Extension Attribute is to list any applications installed in ~/Applications, ~/Desktop, ~/Documents, or ~/Downloads (searches up to three levels deep).

## Extension Attribute:
```
#!/bin/sh

########## variable-ing ##########

loggedInUser=$(/usr/bin/stat -f%Su "/dev/console")
loggedInUserHome=$(/usr/bin/dscl . -read "/Users/${loggedInUser}" NFSHomeDirectory | /usr/bin/awk '{print $NF}')
userApps=""

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

# List all applications found in ~/Applications, ~/Desktop, ~/Documents, or ~/Downloads (if any).
userApps=$(/usr/bin/find "${loggedInUserHome}/Applications" "${loggedInUserHome}/Desktop" "${loggedInUserHome}/Documents" "${loggedInUserHome}/Downloads" -maxdepth 3 -name "*.app" 2>"/dev/null")

# Report results.
echo "<result>${userApps}</result>"

exit 0
```
## Fleet query:
```SELECT name, path, version FROM apps WHERE path LIKE '%/Applications/%' OR path LIKE '%/Desktop/%' OR path LIKE '%/Documents/%' OR path LIKE '%/Downloads/%';```

Compatible with: ✅ macOS ✅ Windows ✅ Linux 🚫 ChromeOS

