# Kernel Extensions (Third-Party)

The purpose of this Extension Attribute is to display all enabled third-party kernel extensions.

## Extension Attribute:
```
#!/bin/sh

########## main process ##########

# Report results.
echo "<result>$(/usr/bin/kmutil showloaded --list-only 2>"/dev/null" | /usr/bin/grep -v 'com.apple' | /usr/bin/awk '{print $6}' | /usr/bin/sort)</result>"

exit 0
```
## Fleet query:
```SELECT name, path, version FROM kernel_extensions WHERE name NOT LIKE 'com.apple.%' ORDER BY name;```

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS

