# System Extensions

The purpose of this Extension Attribute is to list all enabled system extensions.

## Extension Attribute:
```
#!/bin/sh

########## main process ##########

# Report results.
echo "<result>$(/usr/bin/systemextensionsctl list | /usr/bin/awk '/ enabled/ {print $4}' | /usr/bin/sort)</result>"

exit 0
```
## Fleet query:
```SELECT path, filename FROM file WHERE path LIKE '%/SystemExtensions/%' OR path LIKE '%/Library/SystemExtensions/%';```

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS

