# ADB `am` (Activity Manager) Cheatsheet

`adb shell am` is a powerful Android debugging tool used to **start activities**, **broadcast intents**, **force-stop apps**, and more.

---

## General Syntax


> adb shell am [command] [options]
> root#> am start-activity 

### Common Commands
---
- `start` — Start an activity
- `startservice` — Start a service
- `broadcast` — Send a broadcast
- `force-stop` — Force stop a package
- `kill` — Kill all background processes for a package
- `instrument` — Run instrumentation
- `stack` — Manage task stack (advanced)
- `monitor` — Start monitoring (deprecated)


### Start an Activity

``` shell
adb shell am start -n <package>/<activity>
adb shell am start -n com.android.settings/.Settings
# starting with intent details
adb shell am start -n <package>/<activity> --es <Intent Parameter> <Intent Parameter value>
adb shell am start -n com.android.settins/.Settings --es "username" "admin"
# Starting activity for implicit intent
adb shell am start -a <intent action> -c <intent category>
adb shell am start -a com.apphacking.changepin -c android.intent.category.DEFAULT 
adb shell am start -a <intent action> -c <intent category>  --es <Intent Parameter> <Intent Parameter value>
adb shell am start -a com.apphacking.changepin -c android.intent.category.DEFAULT --es "username" "admin"
```
<details>
<summary><strong>All <code>am start-activity</code> Options</strong></summary>

<br>

| Option                            | Description                                              |
|-----------------------------------|----------------------------------------------------------|
| `-D`                              | Enable debugging                                         |
| `-N`                              | Enable native debugging                                  |
| `-W`                              | Wait for launch to complete                              |
| `-P <FILE>`                       | Stop profiling when app goes idle                        |
| `--start-profiler <FILE>`         | Start profiler and send results to file                  |
| `--sampling INTERVAL`             | Use sample profiling with given microsecond interval     |
| `--streaming`                     | Stream profiling output to file                          |
| `--attach-agent <agent>`          | Attach the given agent before binding                    |
| `--attach-agent-bind <agent>`     | Attach the agent during bind phase                       |
| `--track-allocation`              | Track memory allocations                                 |
| `--user <USER_ID>` or `current`   | Specify user context                                     |
| `-S`                              | Stop target app before starting activity                 |
| `--display <DISPLAY_ID>`          | Launch on specific display                               |
| `--activity-brought-to-front`     | Consider as brought to front                             |
| `--activity-clear-top`            | Clear above if activity already exists                   |
| `--activity-clear-when-task-reset`| Clear when task is reset                                 |
| `--activity-exclude-from-recents` | Exclude from recent apps list                            |
| `--activity-launched-from-history`| Relaunch from history                                    |
| `--activity-multiple-task`        | Allow multiple instances in separate tasks               |
| `--activity-no-animation`         | Disable transition animations                            |
| `--activity-no-history`           | Do not keep activity in back stack                       |
| `--activity-no-user-action`       | Don’t record as user action                              |
| `--activity-previous-is-top`      | Previous activity is on top                              |
| `--activity-reorder-to-front`     | Reorder existing instance to front                       |
| `--activity-reset-task-if-needed` | Reset task before starting activity                      |
| `--activity-single-top`           | Reuse if already on top                                  |
| `--activity-task-on-home`         | Place activity’s task on home screen                     |
| `--activity-launch-adjacent`      | Launch in adjacent window if possible                    |

</details>


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

#### Helpful Flags for Activities

- --activity-clear-top — If activity exists, discard above it
- --activity-new-task — Start in a new task
- --activity-no-history — Do not keep in back stack
- --activity-single-top — Reuse top instance if already on top

#### Useful for Security Testing

- Exported Activity Enumeration — adb shell pm dump <package>
- Launch suspicious exported activity — adb shell am start -n <package>/<activity>
- Exploit Intent Extras — Use flags like --es, --ei, --ez, --esa, etc.

## Notes

Make sure the device is connected via USB or WiFi (adb devices)
Use adb shell for interactive commands
Use pm list packages, pm path, or pm dump to explore apps

📎 References

- Android Developers: am Tool
- `adb shell am --help (on-device CLI reference)
