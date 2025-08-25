# adb-cheatsheat
Contains all ADB commands

#### Basic commands
Normal user shell access
```
adb shell
```

Root user shell access
```
adb root
adb shell
```

Root  user access from shell
```
adb shell
su
```

Install APKs
```
adb install <apkname>
```

Uninstall APKs
```
adb uninstall <apkname>
```

Uploading files or folders
```
adb push <sourcepath> <destinationpath>
```

Downloading files or folders
```
adb pull <sourcepath> <destinationpath>
```

Listing installed packages; Here pm means Package manager
```
adb shell pm list packages
```

Getting bask apk path or source apks path
```
adb shell pm path <packagename>
```

#### Activity, Service & Broadcast handling
**-n** component name(Pull path to activity)
```
adb shell am start-activity -n packagename/activityname
```
Example:
```
adb shell am start-activity -n com.android.insecurebankv2/com.android.insecurebankv2.PostLogin
                        [OR]
adb shell am start-activity -n com.android.insecurebankv2/.PostLogin
```
