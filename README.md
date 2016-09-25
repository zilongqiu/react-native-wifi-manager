## WiFi Manager for React Native (react-native-wifi-manager)

List, connect and get the status of the WiFi connection on the device.

## Installation

First you need to install react-native-wifi-manager:

```javascript
"dependencies": {
  ...
  "react-native-wifi-manager": "git+ssh://git@github.com/zilongqiu/react-native-wifi-manager.git#master",
},
```

* In `android/app/src/main/AndroidManifest.xml` add these permissions inside `<manifest/>`.

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.CHANGE_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

* In `android/setting.gradle`

```gradle
...
include ':WifiManager', ':app'
project(':WifiManager').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-wifi-manager/android')
```

* In `android/app/build.gradle`

```gradle
...
dependencies {
    ...
    compile project(':WifiManager')
}
```

* register module (in MainApplication.java)

On newer versions of React Native (0.18+):

```java
import com.skierkowski.WifiManager.*;  // <--- import

public class MainApplication extends Application implements ReactApplication {
  ......

  /**
   * A list of packages used by the app. If the app uses additional views
   * or modules besides the default ones, add more packages here.
   */
    @Override
    protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
        new WifiManager(), // <------ add here
        new MainReactPackage());
    }
}
```

## Example

### Load module
```javascript
var WifiManager = require('react-native-wifi-manager');
```

### List available networks (list)
```javascript
loadWifiListData: function() {
    WifiManager.list(
        (wifiArray) => {
            this.setState({
                // wifiArray is an array of strings, each string being the SSID
                dataSource: this.state.dataSource.cloneWithRows(wifiArray),
            });
        },
        (msg) => {
            console.log(msg);
        },
    );
},
```

### Connect to a new network (connect)
```javascript
// Attempts to connect to the network specified. This is an async call. Listen to connectionStatus for status
WifiManager.connect(ssid,password);
```

### Get status of connection (status)
```javascript
checkConnectionStatus: function() {
    // Possible States: 'CONNECTED', 'CONNECTING', 'DISCONNECTED', 'DISCONNECTING', 'SUSPENDED', 'UNKOWN'
    // from: http://developer.android.com/reference/android/net/NetworkInfo.State.html
    WifiManager.status((status) => {
        if (status == 'CONNECTED') {
            this.navigateToActivation();
        }
    });
},
  ```
