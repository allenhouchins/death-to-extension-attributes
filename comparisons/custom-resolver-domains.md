# Custom Resolver Domains

The purpose of this Extension Attribute is to return a list of custom domains configured in /etc/resolver/.

## Extension Attribute:
```
#!/bin/sh

########## variable-ing ##########

macosResolverPath="/etc/resolver"

########## main process ##########

# List custom domain entries.
if [ -d "$macosResolverPath" ]; then
  resolverDomains=$(/bin/ls -1 "$macosResolverPath")
else
  resolverDomains=""
fi

# Report result.
echo "<result>$resolverDomains</result>"

exit 0
```
## Fleet query:
```SELECT path, filename FROM file WHERE path LIKE '/etc/resolver/%' AND type = 'regular';```

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS

