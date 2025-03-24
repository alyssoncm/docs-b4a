~~~javascript
// NodeJS
npm install parse --save
// Import Parse SDK
const Parse = require('parse/node');
// Javascript (Web)
<script type="text/javascript" src="https://unpkg.com/parse/dist/parse.min.js"></script>
// Initialize with your Back4app keys
Parse.initialize("YOUR_APP_ID", "YOUR_JS_KEY");  // Replace with your App ID and JS Key
Parse.serverURL = 'https://parseapi.back4app.com';
~~~
~~~javascript
// React Native
// Install Parse and AsyncStorage for React Native
npm install parse @react-native-async-storage/async-storage --save
// Run pod install for iOS
cd ios && pod install
// Import Parse SDK
import Parse from 'parse/react-native';
import AsyncStorage from '@react-native-async-storage/async-storage';
// Set AsyncStorage
Parse.setAsyncStorage(AsyncStorage);
// Initialize Parse
Parse.initialize("YOUR_APP_ID", "YOUR_JS_KEY");  // Replace with your App ID and JS Key
Parse.serverURL = 'https://parseapi.back4app.com';
~~~
~~~dart
// Flutter
dependencies:
  parse_server_sdk_flutter: ^latest_version
// Import Parse SDK
import 'package:parse_server_sdk_flutter/parse_server_sdk_flutter.dart';
// Initialize Parse SDK
void main() async {
  const keyApplicationId = 'YOUR_APP_ID_HERE';
  const keyClientKey = 'YOUR_CLIENT_KEY_HERE';
  const keyParseServerUrl = 'https://parseapi.back4app.com';
  await Parse().initialize(keyApplicationId, keyParseServerUrl,
    clientKey: keyClientKey, debug: true);
}
~~~
~~~java
// Android
repositories {
    mavenCentral()
    jcenter()
    maven { url 'https://jitpack.io' }
}
dependencies {
    implementation 'com.github.parse-community.Parse-SDK-Android:parse:latest_version'
}
// Import Parse SDK
import com.parse.Parse;
// Initialize Parse SDK
public class MyApp extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        Parse.initialize(new Parse.Configuration.Builder(this)
            .applicationId("YOUR_APP_ID")
            .clientKey("YOUR_CLIENT_KEY")
            .server("https://parseapi.back4app.com")
            .build()
        );
    }
}
~~~
~~~swift
// Install via Swift Package Manager (SPM) in Xcode:
// File > Add Packages... > https://github.com/parse-community/Parse-Swift
// Import the Parse SDK
import ParseSwift
// Initialize Parse SDK
@main
struct MyApp: App {
    init() {
        ParseSwift.initialize(
            applicationId: "YOUR_APP_ID",
            clientKey: "YOUR_CLIENT_KEY",
            serverURL: URL(string: "https://parseapi.back4app.com")!
        )
    }
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
~~~
~~~php
// PHP
{
  "require": {
    "parse/php-sdk": "*"
  }
}
// Do not forget to run the following command, afterward:
//composer install
// Import Parse SDK
use Parse\ParseClient;
ParseClient::initialize('YOUR_APP_ID', 'YOUR_REST_KEY', 'YOUR_MASTER_KEY');
ParseClient::setServerURL('https://parseapi.back4app.com', '/');
~~~










