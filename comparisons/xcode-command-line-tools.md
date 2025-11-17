# Xcode Command Line Tools

The purpose of this Extension Attribute is to return whether Xcode Command Line Tools are installed (either standalone or as part of Xcode.app bundle).

## Extension Attribute:
```
#!/bin/sh

########## variable-ing ##########

xcodeCLTCheck=""
xcodeAppPath="/Applications/Xcode.app/Contents/Developer"
xcodeCLTPath="/Library/Developer/CommandLineTools"
xcodeCheck=$(/usr/bin/xcode-select --print-path 2>&1)

########## main process ##########

# Check for presence of target file path.
if [ "$xcodeCheck" = "$xcodeAppPath" ] && [ -e "$xcodeAppPath" ]; then
  xcodeCLTCheck="Bundled with Xcode"
elif [ "$xcodeCheck" = "$xcodeCLTPath" ] && [ -e "$xcodeCLTPath" ]; then
  xcodeCLTCheck="Standalone"
fi

# Report result.
echo "<result>$xcodeCLTCheck</result>"

exit 0
```
## Fleet query:
```SELECT path FROM file WHERE path = '/Library/Developer/CommandLineTools' OR path LIKE '/Applications/Xcode.app/Contents/Developer%' LIMIT 1;```

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS

