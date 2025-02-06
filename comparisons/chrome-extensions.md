# Chrome Extensions

This attribute lists all installed Chrome extensions on a host.
 
## Extension Attribute:
```
#!/bin/bash
# Setting IFS Env to only use new lines as field seperator  
IFS=$'\n' 

currentUser=`ls -l /dev/console | awk {' print $3 '}` 
lastUser=`defaults read /Library/Preferences/com.apple.loginwindow lastUserName` 
 
if [[ "$currentUser" = "" || "$currentUser" = "root" ]] 
	then userHome=`/usr/bin/dscl . -read /Users/$lastUser NFSHomeDirectory | awk -F ": " '{print $2}'` 
	else userHome=`/usr/bin/dscl . -read /Users/$currentUser NFSHomeDirectory | awk -F ": " '{print $2}'` 
fi 
 
createChromeExtList () 
{ 
for manifest in $(find "$userHome/Library/Application Support/Google/Chrome/Default/Extensions" -name 'manifest.json') 
	do  
		name=$(cat $manifest | grep '"name":' | awk -F "\"" '{print $4}') 
		if [[ `echo $name | grep "__MSG"` ]] 
			then 
				msgName="\"`echo $name | awk -F '__MSG_|__' '{print $2}'`\":" 
				if [ -f $(dirname $manifest)/_locales/en/messages.json ] 
					then reportedName=$(cat $(dirname $manifest)/_locales/en/messages.json | grep -i -A 3 "$msgName" | grep "message" | head -1 | awk -F ": " '{print $2}' | tr -d "\"") 
				elif [ -f $(dirname $manifest)/_locales/en_US/messages.json ] 
					then reportedName=$(cat $(dirname $manifest)/_locales/en_US/messages.json | grep -i -A 3 "$msgName" | grep "message" | head -1 | awk -F ": " '{print $2}' | tr -d "\"") 
				fi 
			else 
				reportedName=$(cat $manifest | grep '"name":' | awk -F "\"" '{print $4}') 
		fi 
		version=$(cat $manifest | grep '"version":' | awk -F "\"" '{print $4}') 
		extID=$(basename $(dirname $(dirname $manifest))) 
		 
		# This is the default output style - looks nice in JSS 
		# Comment out line below if you wish to use alternate output 
	 	echo -e "Name: $reportedName \nVersion: $version \nID: $extID \n" 
	 	 
	 	# This is the alternate output style - looks ugly in JSS, but possibly more useful 
	 	# Uncomment line below to use this output instead 
	 	#echo -e "$reportedName;$version;$extID" 
 done 
 } 
 
if [ -d "$userHome/Library/Application Support/Google/Chrome/Default/Extensions" ] 
        then result="`createChromeExtList`" 
        else result="NA" 
fi 
 
echo "&lt;result&gt;$result&lt;/result&gt;"</scriptContentsMac>
<scriptContentsWindows/>
</extensionAttribute>
```
## Fleet query:
```SELECT * FROM users CROSS JOIN chrome_extensions USING (uid);```

Because browser data lives in user space, querying requires joining against the users table.

Compatible with: ✅ macOS ✅ Windows ✅ Linux ✅ ChromeOS
