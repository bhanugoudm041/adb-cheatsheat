# adb-cheatsheat
Contains all ADB commands

## Basic commands
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

## Activity, Service & Broadcast handling
### Activity
**-n** component name(Pull path to activity). This below commands send Intent to launch activities
```
adb shell am start-activity -n packagename/activityname
```
Example:
```
adb shell am start-activity -n com.android.insecurebankv2/com.android.insecurebankv2.PostLogin
                        [OR]
adb shell am start-activity -n com.android.insecurebankv2/.PostLogin
```

##### Intent options with Activities
Combining Action with Activity -a android.intent.action.VIEW 
```
adb shell am start-activity -a android.intent.action.VIEW -n com.android.insecurebankv2/.PostLogin
```

Sending strings via extra strings with Intent
```
--es key "value" or -e key "value"
adb shell am start-activity -n com.android.insecurebankv2/.PostLogin --es reg_url "https://3kal.medium.com"
```

Sending data via data URI with Intent
```
-d <DATA_URI>
adb shell am start-activity -n com.android.insecurebankv2/.PostLogin -d "https://hello?abc=hii"
```

### Broadcast
Sending strings via Intent with broadcast receivers
```
adb shell am broadcast -n com.android.insecurebankv2/.BrReciever -a android.intent.action.ACTION_SEND -e uid appsec
```

### Services exploit
