# Mac App Store Apps

The purpose of this Extension Attribute is to list all apps in /Applications downloaded from the Mac App Store.

## Extension Attribute:
```
#!/bin/sh

########## main process ##########

# Display alpha-sorted list of all apps in /Applications with Mac App Store receipts.
echo "<result>$(/usr/bin/mdfind -onlyin "/Applications/" 'kMDItemAppStoreHasReceipt == "1"' | /usr/bin/sort)</result>"

exit 0
```
## Fleet query:
```SELECT name, path, bundle_identifier FROM apps WHERE path LIKE '/Applications/%' AND bundle_identifier IS NOT NULL;```

Compatible with: ✅ macOS 🚫 Windows 🚫 Linux 🚫 ChromeOS

