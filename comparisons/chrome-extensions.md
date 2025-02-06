# Chrome Extensions

This attribute lists all installed Chrome extensions on a host.
 
## Extension Attribute:
```
#!/bin/bash
# Setting IFS Env to only use new lines as field separator  
IFS=$'\n'

currentUser=$(ls -l /dev/console | awk '{ print $3 }')
lastUser=$(defaults read /Library/Preferences/com.apple.loginwindow lastUserName)

if [[ -z "$currentUser" || "$currentUser" == "root" ]]; then
    userHome=$(/usr/bin/dscl . -read /Users/"$lastUser" NFSHomeDirectory | awk -F ": " '{print $2}')
else
    userHome=$(/usr/bin/dscl . -read /Users/"$currentUser" NFSHomeDirectory | awk -F ": " '{print $2}')
fi

if [[ -z "$userHome" ]]; then
    echo "Error: Unable to determine user home directory" >&2
    exit 1
fi

createChromeExtList() {
    for manifest in $(find "$userHome/Library/Application Support/Google/Chrome/Default/Extensions" -name 'manifest.json' 2>/dev/null); do
        name=$(awk -F "\"" '/"name":/ {print $4}' "$manifest")

        if [[ "$name" =~ "__MSG_" ]]; then
            msgName="\"$(echo "$name" | awk -F '__MSG_|__' '{print $2}')\":"

            if [ -f "$(dirname "$manifest")/_locales/en/messages.json" ]; then
                reportedName=$(awk -F ": " -v msg="$msgName" '$0 ~ msg {getline; print $2}' "$(dirname "$manifest")/_locales/en/messages.json" | tr -d "\"")
            elif [ -f "$(dirname "$manifest")/_locales/en_US/messages.json" ]; then
                reportedName=$(awk -F ": " -v msg="$msgName" '$0 ~ msg {getline; print $2}' "$(dirname "$manifest")/_locales/en_US/messages.json" | tr -d "\"")
            fi

            # Fallback if localization lookup fails
            if [[ -z "$reportedName" ]]; then
                reportedName="$name"
            fi
        else
            reportedName="$name"
        fi

        version=$(awk -F "\"" '/"version":/ {print $4}' "$manifest")
        extID=$(basename "$(dirname "$(dirname "$manifest")")")

        # Default output style
        echo -e "Name: \"$reportedName\" \nVersion: \"$version\" \nID: \"$extID\" \n"

        # Alternative output style (Uncomment to use)
        # echo -e "$reportedName;$version;$extID"
    done
}

if [ -d "$userHome/Library/Application Support/Google/Chrome/Default/Extensions" ]; then
    result=$(createChromeExtList)
else
    result="NA"
fi

echo "<result>$result</result>"
```
## Fleet query:
```SELECT * FROM users CROSS JOIN chrome_extensions USING (uid);```

Because browser data lives in user space, querying requires joining against the users table.

Compatible with: ✅ macOS ✅ Windows ✅ Linux ✅ ChromeOS
