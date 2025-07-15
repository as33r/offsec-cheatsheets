# ADB `am` (Activity Manager) Cheatsheet

`adb shell am` is a powerful Android debugging tool used to **start activities**, **broadcast intents**, **force-stop apps**, and more.

---

## General Syntax


> adb shell am [command] [options]
> root#> am start-activity 

### Common Commands
---
`Command	              Description
`start	                  Start an activity
`startservice	          Start a service
`broadcast	              Send a broadcast
`force-stop	              Force stop a package
`kill	                  Kill all background processes for a package
`instrument	              Run instrumentation
`stack	                  Manage task stack (advanced)
`monitor	              Start monitoring (deprecated)

### Start an Activity

``` shell
adb shell am start -n <package>/<activity>
adb shell am start -n com.android.settings/.Settings
```

### Start an Activity with URL
``` shell
adb shell am start -a android.intent.action.VIEW -d <url>

#Example:
adb shell am start -a android.intent.action.VIEW -d https://example.com
```

### Send a Broadcast Intent
``` shell
adb shell am broadcast -a <action> [--ez|--es|--ei|--esn ...]

#Example:
adb shell am broadcast -a android.intent.action.BOOT_COMPLETED

#With extras:
adb shell am broadcast -a com.test.ACTION --es "key" "value"
```

### Start a Service
``` shell
adb shell am startservice -n <package>/<service>

#Example:
adb shell am startservice -n com.example/.MyService
```

### Force Stop an App
``` shell
adb shell am force-stop <package>

#Example:
adb shell am force-stop com.facebook.katana
```

### Kill Background Processes
``` shell
adb shell am kill <package>
#Kills background process but doesn't remove app from recent apps.
```
### Run Instrumentation Test
``` shell
adb shell am instrument -w <test_package>/<runner_class>

#Example:
adb shell am instrument -w com.example.test/androidx.test.runner.AndroidJUnitRunner
```
### Start with Intent Details
``` shell
adb shell am start \
    -a <action> \
    -d <data-uri> \
    -t <mime-type> \
    -c <category> \
    -n <component> \
    --esn <extra-null> \
    --es <extra-string> \
    --ei <extra-int> \
    --ez <extra-boolean> \
    --grant-read-uri-permission \
    --activity-clear-top \
    --activity-new-task

 # Example:

adb shell am start \
  -a android.intent.action.VIEW \
  -d "content://contacts/people/1" \
  -t "vnd.android.cursor.item/person"
```

### Helpful Flags for Activities

`Flag	                Description
`--activity-clear-top	If activity exists, discard above it
`--activity-new-task	Start in a new task
`--activity-no-history	Do not keep in back stack
`--activity-single-top	Reuse top instance if already on top

### Useful for Security Testing

`Use Case	                          Command
`Exported Activity Enumeration	`        `adb shell pm dump <pkg>`
`Launch suspicious exported activity`  `adb shell am start -n <pkg>/<activity>
`Exploit Intent Extras            	--es, --ei, --ez, etc.`

## Notes

Make sure the device is connected via USB or WiFi (adb devices)
Use adb shell for interactive commands
Use pm list packages, pm path, or pm dump to explore apps

📎 References

- Android Developers: am Tool
- `adb shell am --help (on-device CLI reference)
