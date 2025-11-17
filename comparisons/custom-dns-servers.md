# Custom DNS Servers

The purpose of this Extension Attribute is to list all network interfaces with custom DNS server entries.

## Extension Attribute:
```
#!/bin/sh

########## variable-ing ##########

outputPath="/tmp/dnsservers.txt"

########## main process ##########

# Initialize output file.
if [ -e "$outputPath" ]; then
  /bin/rm "$outputPath"
fi
/usr/bin/touch "$outputPath"

# Collect all custom DNS servers found in each network interface and pass to an
# output file for later reading.
/usr/sbin/networksetup -listallnetworkservices | /usr/bin/sed 1d | /usr/bin/tr -d "*" | while read -r interface; do
  dnsServerList=$(/usr/sbin/networksetup -getdnsservers "$interface")
  if [ "$dnsServerList" != "There aren't any DNS Servers set on ${interface}." ]; then
    dnsServerList=$(echo "$dnsServerList" | /usr/bin/tr '\n' ',' | /usr/bin/sed 's/,$//')
    echo "${interface}: ${dnsServerList}" >> "$outputPath"
  fi
done

# Report results.
echo "<result>$(/bin/cat "${outputPath}")</result>"

# Clean up.
/bin/rm "$outputPath"

exit 0
```
## Fleet query:
```SELECT interface, nameservers FROM dns_resolvers WHERE interface != 'lo0' ORDER BY interface;```

Compatible with: ✅ macOS ✅ Windows ✅ Linux 🚫 ChromeOS

