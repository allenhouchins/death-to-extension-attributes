# Java Version

The purpose of this Extension Attribute is to return Java version(s) if installed.

## Extension Attribute:
```
#!/bin/sh

########## variable-ing ##########

javaVMPath="/Library/Java/JavaVirtualMachines"
javaVersionListTempFile="/tmp/javaVersionList.txt"

########## main process ##########

# Initialize temp file.
if [ -e "$javaVersionListTempFile" ]; then
  /bin/rm "$javaVersionListTempFile"
fi
/usr/bin/touch "$javaVersionListTempFile"

# Check for presence of Java install(s) and get version(s).
if [ -d "$javaVMPath" ]; then
  /usr/bin/find "$javaVMPath" -maxdepth 1 -name "*.jdk" | /usr/bin/sort | while read -r javaInstall; do
    javaBinPath="$javaInstall/Contents/Home/bin/java"
    if [ -e "$javaBinPath" ]; then
      javaVersion=$("$javaBinPath" -version 2>&1 | /usr/bin/awk '/version/ {print}')
      echo "$javaVersion" >> "$javaVersionListTempFile"
    fi
  done
fi

# Report result.
echo "<result>$(/bin/cat $javaVersionListTempFile)</result>"

exit 0
```
## Fleet query:
```SELECT name, path, version FROM apps WHERE path LIKE '%/JavaVirtualMachines/%' OR name LIKE '%Java%';```

Compatible with: ✅ macOS ✅ Windows ✅ Linux 🚫 ChromeOS

