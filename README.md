# Parse SDK Installation and Initialization Guide

This guide provides step-by-step instructions for installing and initializing Parse SDK across different platforms.

## JavaScript (Browser)

### Step 1: Install Parse SDK
```html
<!-- Include Parse SDK via CDN -->
<script type="text/javascript" src="https://unpkg.com/parse/dist/parse.min.js"></script>
```

### Step 2: Initialize Parse SDK
```javascript
// Initialize with your Back4app keys
Parse.initialize("YOUR_APP_ID", "YOUR_JS_KEY");  // Replace with your App ID and JS Key
Parse.serverURL = 'https://parseapi.back4app.com';
```

### Basic CRUD Operations
```javascript
// Create a new object
const GameScore = Parse.Object.extend("GameScore");
const gameScore = new GameScore();
gameScore.set("score", 1337);
gameScore.set("playerName", "Sean Plott");
gameScore.set("cheatMode", false);
gameScore.save()
  .then((gameScore) => {
    console.log('New object created with objectId: ' + gameScore.id);
  }, (error) => {
    console.log('Failed to create new object, with error code: ' + error.message);
  });

// Query objects
const GameScore = Parse.Object.extend("GameScore");
const query = new Parse.Query(GameScore);
query.greaterThan("score", 1000);
query.find()
  .then((results) => {
    console.log("Successfully retrieved " + results.length + " scores.");
    // Do something with the returned Parse.Object values
    for (let i = 0; i < results.length; i++) {
      const object = results[i];
      console.log(object.id + ' - ' + object.get('playerName'));
    }
  }, (error) => {
    console.log("Error: " + error.code + " " + error.message);
  });

// Update an object
const query = new Parse.Query(GameScore);
query.get("xWMyZ4YEGZ")
  .then((gameScore) => {
    // The object was retrieved successfully.
    gameScore.set("score", 1338);
    return gameScore.save();
  })
  .then((gameScore) => {
    console.log('Object updated successfully');
  }, (error) => {
    console.log("Error: " + error.code + " " + error.message);
  });

// Delete an object
const query = new Parse.Query(GameScore);
query.get("xWMyZ4YEGZ")
  .then((gameScore) => {
    return gameScore.destroy();
  })
  .then(() => {
    console.log('Object deleted successfully');
  }, (error) => {
    console.log("Error: " + error.code + " " + error.message);
  });
```

### Basic CRUD Operations
```javascript
// Create a new object
const GameScore = Parse.Object.extend("GameScore");
const gameScore = new GameScore();
gameScore.set("score", 1337);
gameScore.set("playerName", "Sean Plott");
gameScore.set("cheatMode", false);
gameScore.save()
  .then((gameScore) => {
    console.log('New object created with objectId: ' + gameScore.id);
  }, (error) => {
    console.log('Failed to create new object, with error code: ' + error.message);
  });

// Query objects
const GameScore = Parse.Object.extend("GameScore");
const query = new Parse.Query(GameScore);
query.greaterThan("score", 1000);
query.find()
  .then((results) => {
    console.log("Successfully retrieved " + results.length + " scores.");
    // Do something with the returned Parse.Object values
    for (let i = 0; i < results.length; i++) {
      const object = results[i];
      console.log(object.id + ' - ' + object.get('playerName'));
    }
  }, (error) => {
    console.log("Error: " + error.code + " " + error.message);
  });

// Update an object
const query = new Parse.Query(GameScore);
query.get("xWMyZ4YEGZ")
  .then((gameScore) => {
    // The object was retrieved successfully.
    gameScore.set("score", 1338);
    gameScore.save();
  }, (error) => {
    console.log("Error: " + error.code + " " + error.message);
  });

// Delete an object
const query = new Parse.Query(GameScore);
query.get("xWMyZ4YEGZ")
  .then((gameScore) => {
    gameScore.destroy();
  }, (error) => {
    console.log("Error: " + error.code + " " + error.message);
  });
```

## Node.js

### Step 1: Install Parse SDK
```bash
npm install parse --save
```

### Step 2: Initialize Parse SDK
```javascript
// Import Parse SDK
const Parse = require('parse/node');

// Initialize with your Back4app keys
Parse.initialize("YOUR_APP_ID", "YOUR_JS_KEY");  // Replace with your App ID and JS Key
Parse.serverURL = 'https://parseapi.back4app.com';
```

## React.js / Next.js

### Step 1: Install Parse SDK
```bash
npm install parse --save
```

### Step 2: Initialize Parse SDK
```javascript
// Import Parse SDK
import Parse from 'parse';

// Initialize Parse on component/app mount
function initializeParse() {
  Parse.initialize("YOUR_APP_ID", "YOUR_JS_KEY");  // Replace with your App ID and JS Key
  Parse.serverURL = 'https://parseapi.back4app.com';
}

// For Next.js, you might initialize in a useEffect hook or in _app.js
useEffect(() => {
  initializeParse();
}, []);

// OR in _app.js / App.jsx:
// initializeParse();
```

### Basic CRUD Operations
```javascript
// Create a new object
const GameScore = Parse.Object.extend("GameScore");
const gameScore = new GameScore();
gameScore.set("score", 1337);
gameScore.set("playerName", "Sean Plott");
gameScore.set("cheatMode", false);
await gameScore.save();

// Query objects
const query = new Parse.Query(GameScore);
query.greaterThan("score", 1000);
const results = await query.find();

// Update an object
const objectId = "xWMyZ4YEGZ";
const query = new Parse.Query(GameScore);
const gameScore = await query.get(objectId);
gameScore.set("score", 1338);
await gameScore.save();

// Delete an object
const query = new Parse.Query(GameScore);
const gameScore = await query.get(objectId);
await gameScore.destroy();
```

## React Native

### Step 1: Install Parse SDK
```bash
# Install Parse and AsyncStorage for React Native
npm install parse @react-native-async-storage/async-storage --save

# Run pod install for iOS
cd ios && pod install
```

### Step 2: Initialize Parse SDK
```javascript
// Import Parse SDK
import Parse from 'parse/react-native';
import AsyncStorage from '@react-native-async-storage/async-storage';

// Set AsyncStorage
Parse.setAsyncStorage(AsyncStorage);

// Initialize Parse
Parse.initialize("YOUR_APP_ID", "YOUR_JS_KEY");  // Replace with your App ID and JS Key
Parse.serverURL = 'https://parseapi.back4app.com';
```

### Basic CRUD Operations
```javascript
// Create a new object
async function createObject() {
  const GameScore = Parse.Object.extend("GameScore");
  const gameScore = new GameScore();
  gameScore.set("score", 1337);
  gameScore.set("playerName", "Sean Plott");
  gameScore.set("cheatMode", false);
  
  try {
    const result = await gameScore.save();
    console.log('New object created with objectId: ' + result.id);
    return result;
  } catch (error) {
    console.error('Failed to create new object:', error);
  }
}

// Query objects
async function queryObjects() {
  const GameScore = Parse.Object.extend("GameScore");
  const query = new Parse.Query(GameScore);
  query.greaterThan("score", 1000);
  
  try {
    const results = await query.find();
    console.log("Successfully retrieved " + results.length + " scores");
    return results;
  } catch (error) {
    console.error('Error querying objects:', error);
  }
}

// Update an object
async function updateObject(objectId) {
  const GameScore = Parse.Object.extend("GameScore");
  const query = new Parse.Query(GameScore);
  
  try {
    const gameScore = await query.get(objectId);
    gameScore.set("score", 1338);
    const result = await gameScore.save();
    console.log('Object updated successfully');
    return result;
  } catch (error) {
    console.error('Error updating object:', error);
  }
}

// Delete an object
async function deleteObject(objectId) {
  const GameScore = Parse.Object.extend("GameScore");
  const query = new Parse.Query(GameScore);
  
  try {
    const gameScore = await query.get(objectId);
    await gameScore.destroy();
    console.log('Object deleted successfully');
  } catch (error) {
    console.error('Error deleting object:', error);
  }
}
```

## Flutter

### Step 1: Install Parse SDK
```yaml
# In pubspec.yaml
dependencies:
  parse_server_sdk_flutter: ^latest_version
```

### Step 2: Initialize Parse SDK
```dart
// Import Parse SDK
import 'package:parse_server_sdk_flutter/parse_server_sdk_flutter.dart';

// Initialize Parse SDK
void main() async {
  const keyApplicationId = 'YOUR_APP_ID_HERE';
  const keyClientKey = 'YOUR_CLIENT_KEY_HERE';
  const keyParseServerUrl = 'https://parseapi.back4app.com';
  
  await Parse().initialize(
    keyApplicationId, 
    keyParseServerUrl,
    clientKey: keyClientKey, 
    debug: true
  );
}
```

### Basic CRUD Operations
```dart
// Create a new object
Future<void> createObject() async {
  final gameScore = ParseObject('GameScore')
    ..set('score', 1337)
    ..set('playerName', 'Sean Plott')
    ..set('cheatMode', false);
  
  try {
    final response = await gameScore.save();
    if (response.success) {
      print('Object created successfully: ${response.result.objectId}');
    }
  } catch (e) {
    print('Error creating object: $e');
  }
}

// Query objects
Future<void> queryObjects() async {
  final query = QueryBuilder<ParseObject>(ParseObject('GameScore'))
    ..whereGreaterThan('score', 1000);
  
  try {
    final response = await query.query();
    if (response.success && response.results != null) {
      for (var gameScore in response.results!) {
        print('${gameScore.objectId} - ${gameScore.get<String>('playerName')}');
      }
    }
  } catch (e) {
    print('Error querying objects: $e');
  }
}

// Update an object
Future<void> updateObject(String objectId) async {
  final query = QueryBuilder<ParseObject>(ParseObject('GameScore'))
    ..whereEqualTo('objectId', objectId);
  
  try {
    final response = await query.query();
    if (response.success && response.results != null && response.results!.isNotEmpty) {
      final gameScore = response.results!.first;
      gameScore.set('score', 1338);
      final updateResponse = await gameScore.save();
      if (updateResponse.success) {
        print('Object updated successfully');
      }
    }
  } catch (e) {
    print('Error updating object: $e');
  }
}

// Delete an object
Future<void> deleteObject(String objectId) async {
  final gameScore = ParseObject('GameScore')..objectId = objectId;
  
  try {
    final response = await gameScore.delete();
    if (response.success) {
      print('Object deleted successfully');
    }
  } catch (e) {
    print('Error deleting object: $e');
  }
}
```

## Android (Java/Kotlin)

### Step 1: Install Parse SDK
```groovy
// In build.gradle
repositories {
    mavenCentral()
    jcenter()
    maven { url 'https://jitpack.io' }
}

dependencies {
    implementation 'com.github.parse-community.Parse-SDK-Android:parse:latest_version'
}
```

### Step 2: Initialize Parse SDK
```java
// Import Parse SDK
import com.parse.Parse;

// Initialize Parse SDK (in Application class)
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
```

### Basic CRUD Operations
```java
// Create a new object
public void createObject() {
    ParseObject gameScore = new ParseObject("GameScore");
    gameScore.put("score", 1337);
    gameScore.put("playerName", "Sean Plott");
    gameScore.put("cheatMode", false);
    gameScore.saveInBackground(new SaveCallback() {
        @Override
        public void done(ParseException e) {
            if (e == null) {
                // Success
                Log.d("CREATE", "Object saved: " + gameScore.getObjectId());
            } else {
                // Error
                Log.e("CREATE", "Error: " + e.getMessage());
            }
        }
    });
}

// Query objects
public void queryObjects() {
    ParseQuery<ParseObject> query = ParseQuery.getQuery("GameScore");
    query.whereGreaterThan("score", 1000);
    query.findInBackground(new FindCallback<ParseObject>() {
        @Override
        public void done(List<ParseObject> objects, ParseException e) {
            if (e == null) {
                // Success
                Log.d("QUERY", "Retrieved " + objects.size() + " objects");
                for (ParseObject object : objects) {
                    Log.d("QUERY", object.getObjectId() + " - " + object.getString("playerName"));
                }
            } else {
                // Error
                Log.e("QUERY", "Error: " + e.getMessage());
            }
        }
    });
}

// Update an object
public void updateObject(String objectId) {
    ParseQuery<ParseObject> query = ParseQuery.getQuery("GameScore");
    query.getInBackground(objectId, new GetCallback<ParseObject>() {
        @Override
        public void done(ParseObject object, ParseException e) {
            if (e == null) {
                // Success
                object.put("score", 1338);
                object.saveInBackground(new SaveCallback() {
                    @Override
                    public void done(ParseException e) {
                        if (e == null) {
                            Log.d("UPDATE", "Object updated successfully");
                        } else {
                            Log.e("UPDATE", "Error: " + e.getMessage());
                        }
                    }
                });
            } else {
                // Error
                Log.e("UPDATE", "Error: " + e.getMessage());
            }
        }
    });
}

// Delete an object
public void deleteObject(String objectId) {
    ParseQuery<ParseObject> query = ParseQuery.getQuery("GameScore");
    query.getInBackground(objectId, new GetCallback<ParseObject>() {
        @Override
        public void done(ParseObject object, ParseException e) {
            if (e == null) {
                // Success
                object.deleteInBackground(new DeleteCallback() {
                    @Override
                    public void done(ParseException e) {
                        if (e == null) {
                            Log.d("DELETE", "Object deleted successfully");
                        } else {
                            Log.e("DELETE", "Error: " + e.getMessage());
                        }
                    }
                });
            } else {
                // Error
                Log.e("DELETE", "Error: " + e.getMessage());
            }
        }
    });
}
```

## iOS (Swift)

### Step 1: Install Parse SDK
```swift
// Install via Swift Package Manager (SPM) in Xcode:
// File > Add Packages... > https://github.com/parse-community/Parse-Swift
```

### Step 2: Initialize Parse SDK
```swift
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
```

### Basic CRUD Operations
```swift
// Create a new object
func createObject() {
    var gameScore = GameScore()
    gameScore.score = 1337
    gameScore.playerName = "Sean Plott"
    gameScore.cheatMode = false
    
    gameScore.save { result in
        switch result {
        case .success(let savedScore):
            print("Object created with ID: \(savedScore.objectId!)")
        case .failure(let error):
            print("Error saving: \(error)")
        }
    }
}

// Query objects
func queryObjects() {
    let query = GameScore.query("score" > 1000)
    
    query.find { result in
        switch result {
        case .success(let scores):
            print("Retrieved \(scores.count) scores")
            for score in scores {
                print("\(score.objectId!) - \(score.playerName ?? "unknown")")
            }
        case .failure(let error):
            print("Error querying: \(error)")
        }
    }
}

// Update an object
func updateObject(objectId: String) {
    let query = GameScore.query("objectId" == objectId)
    
    query.first { result in
        switch result {
        case .success(let gameScore):
            if var gameScore = gameScore {
                gameScore.score = 1338
                gameScore.save { saveResult in
                    switch saveResult {
                    case .success:
                        print("Object updated successfully")
                    case .failure(let error):
                        print("Error updating: \(error)")
                    }
                }
            }
        case .failure(let error):
            print("Error fetching: \(error)")
        }
    }
}

// Delete an object
func deleteObject(objectId: String) {
    let query = GameScore.query("objectId" == objectId)
    
    query.first { result in
        switch result {
        case .success(let gameScore):
            if let gameScore = gameScore {
                gameScore.delete { deleteResult in
                    switch deleteResult {
                    case .success:
                        print("Object deleted successfully")
                    case .failure(let error):
                        print("Error deleting: \(error)")
                    }
                }
            }
        case .failure(let error):
            print("Error fetching: \(error)")
        }
    }
}
```

## PHP

### Step 1: Install Parse SDK
```json
// In composer.json
{
  "require": {
    "parse/php-sdk": "*"
  }
}
```
```bash
# Terminal command to install
composer install
```

### Step 2: Initialize Parse SDK
```php
// Import Parse SDK
use Parse\ParseClient;

// Initialize Parse SDK
ParseClient::initialize('YOUR_APP_ID', 'YOUR_REST_KEY', 'YOUR_MASTER_KEY');
ParseClient::setServerURL('https://parseapi.back4app.com', '/');
```

### Basic CRUD Operations
```php
// Create a new object
function createObject() {
    $gameScore = new ParseObject("GameScore");
    $gameScore->set("score", 1337);
    $gameScore->set("playerName", "Sean Plott");
    $gameScore->set("cheatMode", false);
    
    try {
        $gameScore->save();
        echo 'New object created with objectId: ' . $gameScore->getObjectId() . "\n";
    } catch (ParseException $ex) {
        echo 'Failed to create new object, with error message: ' . $ex->getMessage() . "\n";
    }
}

// Query objects
function queryObjects() {
    $query = new ParseQuery("GameScore");
    $query->greaterThan("score", 1000);
    
    try {
        $results = $query->find();
        echo "Successfully retrieved " . count($results) . " scores.\n";
        
        foreach ($results as $object) {
            echo $object->getObjectId() . ' - ' . $object->get("playerName") . "\n";
        }
    } catch (ParseException $ex) {
        echo 'Failed to retrieve objects, with error message: ' . $ex->getMessage() . "\n";
    }
}

// Update an object
function updateObject($objectId) {
    $query = new ParseQuery("GameScore");
    
    try {
        $gameScore = $query->get($objectId);
        $gameScore->set("score", 1338);
        $gameScore->save();
        echo "Object updated successfully\n";
    } catch (ParseException $ex) {
        echo 'Failed to update object, with error message: ' . $ex->getMessage() . "\n";
    }
}

// Delete an object
function deleteObject($objectId) {
    $query = new ParseQuery("GameScore");
    
    try {
        $gameScore = $query->get($objectId);
        $gameScore->destroy();
        echo "Object deleted successfully\n";
    } catch (ParseException $ex) {
        echo 'Failed to delete object, with error message: ' . $ex->getMessage() . "\n";
    }
}
```

## REST API Quick Reference

No SDK installation required. Use standard HTTP requests with appropriate headers.

### Authentication Headers
```
X-Parse-Application-Id: YOUR_APP_ID
X-Parse-REST-API-Key: YOUR_REST_API_KEY
Content-Type: application/json
```

### Example: Fetch Objects
```bash
curl -X GET \
  -H "X-Parse-Application-Id: YOUR_APP_ID" \
  -H "X-Parse-REST-API-Key: YOUR_REST_API_KEY" \
  https://parseapi.back4app.com/classes/GameScore
```

### Example: Create Object
```bash
curl -X POST \
  -H "X-Parse-Application-Id: YOUR_APP_ID" \
  -H "X-Parse-REST-API-Key: YOUR_REST_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"score":1337,"playerName":"Sean Plott","cheatMode":false}' \
  https://parseapi.back4app.com/classes/GameScore
```

### Example: Update Object
```bash
curl -X PUT \
  -H "X-Parse-Application-Id: YOUR_APP_ID" \
  -H "X-Parse-REST-API-Key: YOUR_REST_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"score":1338}' \
  https://parseapi.back4app.com/classes/GameScore/OBJECT_ID
```

## GraphQL API Quick Reference

### Step 1: Enable GraphQL on your Parse Server
Make sure GraphQL is enabled in your Parse Server configuration.

### Step 2: Use GraphQL Endpoint
The GraphQL endpoint is typically at:
```
https://parseapi.back4app.com/graphql
```

### Authentication Headers
```
X-Parse-Application-Id: YOUR_APP_ID
X-Parse-Client-Key: YOUR_CLIENT_KEY
```

### Example: Query Objects
```graphql
query {
  gameScores {
    edges {
      node {
        objectId
        playerName
        score
        createdAt
      }
    }
  }
}
```

### Example: Create Object (Mutation)
```graphql
mutation CreateGameScore {
  createGameScore(
    input: {
      fields: {
        playerName: "John Doe"
        score: 1200
        cheatMode: false
      }
    }
  ) {
    gameScore {
      objectId
      createdAt
    }
  }
}
```

---

Remember to replace `YOUR_APP_ID`, `YOUR_JS_KEY`, `YOUR_CLIENT_KEY`, etc. with your actual credentials from Back4App or your Parse Server instance.
