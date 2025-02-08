# Connected Displays

This Extension Attribute shows all displays connected to a device.

## Extension Attribute:
```
from __future__ import print_function
import subprocess
import plistlib

def main():
    count_displays = 0
    master_string = ""

    raw_displays = subprocess.check_output(["system_profiler", "SPDisplaysDataType", "-xml"])
    plist_displays = plistlib.loads(raw_displays)
    list_of_displays = plist_displays[0]['_items']

    display_info = []

    for item in list_of_displays:
        sublist = item.get('spdisplays_ndrvs', [])
        count_displays += len(sublist)

        for subitem in sublist:
            feature_list = []

            name_string = subitem.get('_name', "")
            resolution_string = subitem.get('_spdisplays_resolution', "")

            # Append pixel resolution if available
            if '_spdisplays_pixels' in subitem:
                resolution_string += f" ({subitem['_spdisplays_pixels']})"

            display_type = subitem.get('spdisplays_display_type', "")

            # Check for primary display
            if subitem.get('spdisplays_main') == 'yes':
                feature_list.append("Primary")

            # Identify built-in and Retina displays
            if 'built' in display_type:
                feature_list.append("Built-in")

            if 'retina' in display_type:
                feature_list.append("Retina")

            # Identify mirrored displays
            if subitem.get('spdisplays_mirror') == 'spdisplays_on':
                feature_list.append("Mirrored")

            output_string = f"{name_string}, {', '.join(feature_list)}, {resolution_string}".strip(", ")
            display_info.append(output_string)

    master_string = f"{count_displays}: " + "; ".join(display_info) if display_info else "No displays found"
    print(f"<result>{master_string}</result>")

if __name__ == '__main__':
    main()
```
## Fleet query:
```SELECT * FROM connected_displays```

Compatible with: ✅ macOS ✅ Windows ✅ Linux ✅ ChromeOS