# .NET Runtime Versions

The purpose of this Extension Attribute is to report versions of all .NET runtimes installed.

## Extension Attribute:
```
#!/bin/sh

########## variable-ing ##########

loggedInUser=$(/usr/bin/stat -f%Su "/dev/console")
version=""

########## function-ing ##########

# Exits if root is the currently logged-in user, or no logged-in user is detected.
check_logged_in_user () {
  if [ "$loggedInUser" = "root" ] || [ -z "$loggedInUser" ]; then
    echo "Nobody is logged in, no action required."
    exit 0
  fi
}

########## main process ##########

# Check script prerequisites.
check_logged_in_user

# List .NET runtimes and get version strings.
if [ -e "/usr/local/share/dotnet/dotnet" ]; then
  version=$(sudo -u "$loggedInUser" /usr/local/share/dotnet/dotnet --list-runtimes | /usr/bin/awk '/.NETCore.App/ {print $2}')
fi

# Report results.
echo "<result>${version}</result>"

exit 0
```
## Fleet query:
```SELECT name, path, version FROM apps WHERE path LIKE '%/dotnet%' OR name LIKE '%.NET%';```

**Note:** For detailed runtime versions, you may need to check the dotnet binary directly or use a custom query.

Compatible with: ✅ macOS ✅ Windows ✅ Linux 🚫 ChromeOS

