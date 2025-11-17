# Vagrant Version

The purpose of this Extension Attribute is to return Vagrant version if installed.

## Extension Attribute:
```
#!/bin/sh

########## variable-ing ##########

vagrantPath="/opt/vagrant/bin/vagrant"

########## main process ##########

# Check for presence of target binary and get version.
if [ -e "$vagrantPath" ]; then
  vagrantVersion=$("$vagrantPath" version 2>"/dev/null" | /usr/bin/awk '/Installed Version/ {print $3}')
else
  vagrantVersion=""
fi

# Report result.
echo "<result>$vagrantVersion</result>"

exit 0
```
## Fleet query:
```SELECT path, size, mtime FROM file WHERE path = '/opt/vagrant/bin/vagrant';```

Compatible with: ✅ macOS ✅ Windows ✅ Linux 🚫 ChromeOS

