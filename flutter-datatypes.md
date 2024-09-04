---
title: Data Types
slug: flutter/parse-sdk/data-objects/flutter-data-types
description: In this guide you learn about Datatype of Parse using Flutter and how to read and save this data.
image: https://www.back4app.com/images/logo-social.png
createdAt: 2024-03-15T14:18:16.681Z
updatedAt: 2024-08-23T16:20:33.156Z
---

# Parse Datatypes on Flutter

## Introduction

This guide introduces Parse Datatypes in Flutter, using examples from the main.dart file.

Parse, a powerful backend framework, supports various common data types such as strings, numbers, booleans, DateTime, GeoPoints, Pointers, Relations, lists, and objects.

Parse data storage revolves around ParseObject, which contains key-value pairs of JSON-compatible data, making it schemaless. This means you can store any data without defining a schema in advance.

[](https://www.youtube.com/embed/nsF6p2n_Yjo)

## Prerequisites

To complete this tutorial, you will need:

:::hint{type="info"}
- Flutter version 3.x.x or later
- [Android Studio ](https://developer.android.com/studio)or [VS Code installed](https://code.visualstudio.com/) (with [Plugins](https://docs.flutter.dev/get-started/editor) Dart and Flutter)
- An app [created](https://www.back4app.com/docs/get-started/new-parse-app) on Back4App:
  - **Note: **Follow the [New Parse App Tutorial](https://www.back4app.com/docs/get-started/new-parse-app) to learn how to create a Parse App on Back4App.
- A Flutter app connected to Back4app.
  - **Note: **Follow the [Install Parse SDK on Flutter project](https://www.back4app.com/docs/flutter/parse-sdk/parse-flutter-sdk) to create a Flutter Project connected to Back4App.
- A device (or virtual device) running Android or iOS.
:::

## Understanding our App

To better understand Back4app in Flutter, we will create an application that will save and retrieve records with the main types of data supported. We won’t explain the Flutter application code once this guide’s primary focus is using the Flutter with Parse. The data types Pointer, Relation, File, Geopoint will be covered later in specific guides.

## 1 - Working with Parse Objects

Each ParseObject has a class name (e.g., GamePoint) used to distinguish different data types. Here’s how you can create and save a new Parse object:

```dart
final gamePoint = ParseObject('GamePoint')
  ..set('score', 1337)
  ..set('playerName', 'Sean Plott')
  ..set('cheatMode', false);
await gamePoint.save();
```

**Auto-generated fields**:

- 'objectId': Unique identifier for each object.
- 'createdAt': Timestamp of object creation.

**Naming Conventions**:

- Class names: '**NameYourClassesLikeThis**'
- Keys: '**nameYourKeysLikeThis'**

:::hint{type="info"}
We suggest you `NameYourClassesLikeThis` and `nameYourKeysLikeThis` to enhance code readability and maintainability.
:::

## 2 - Counters

You can increment or decrement an integer field in a ParseObject using the setIncrement() and setDecrement() methods.

```dart
1   final todo = ParseObject('Todo')
2     ..objectId = objectId
3     ..set('intField', 3);
4  await todo.save();
```

However, it’s cumbersome and can lead to problems if multiple clients are trying to update the same counter. Parse provides methods that automatically increment (or decrement) any number field to help store counter-type data.&#x20;

- setIncrement()
- setDecrement()

So, the same update can be rewritten to increment as:

```dart
1  final todo = ParseObject('Todo')
2  ..objectId = 'yourObjectId'
3  ..setIncrement('intField', 1);
4 await todo.save();
  
```

To decrement:

```dart
1 final todo = ParseObject('Todo')
2  ..objectId = 'yourObjectId'
3  ..setDecrement('intField', 1);
4 await todo.save();

```

Using setIncrement() with the save() call allows you to increment a value as part of a larger save operation where you may be modifying multiple fields. This is better to avoid an extra network request.

## 3 - Working with Lists

Parse provides methods to work with list data, including setAdd, setAddUnique, setRemove, and their respective All versions.

### **3.1 - Example - "setAdd":**

> final parseObject = ParseObject('DataTypes')
>   ..objectId = 'yourObjectId'
>   ..setAdd('listStringField', 'd');
> await parseObject.save();

**Result:**&#x20;

> \["a","b","c","d"]

### **3.2 - Example - "setAddUnique":**

> final parseObject = ParseObject('DataTypes')
>   ..objectId = 'yourObjectId'
>   ..setAddUnique('listStringField', 'e');
> await parseObject.save();
>

**Result:**

> \["a","b","c","d","d"]

### 3.3 - Example "removeAll"

> final parseObject = ParseObject('DataTypes')
>   ..objectId = 'yourObjectId'
>   ..setAddUnique('listStringField', 'f');
> await parseObject.save();
>

**Result:**

> \["a","b"]

## 4 - Remove field from ParseObject

You can delete a single field from an object by using the unset operation:

> final parseObject = ParseObject('DataTypes')
>   ..unset("stringField");

## 5** - Full Code-Line Example:**

```dart
import 'package:flutter/material.dart';
import 'package:parse_server_sdk_flutter/parse_server_sdk_flutter.dart';
import 'package:image_picker/image_picker.dart';
import 'package:flutter/foundation.dart';

import 'dart:io';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();

  const keyParseApplicationId = 'YOUR_APP_ID';
  const keyParseClientKey = 'YOUR_CLIENT_KEY';
  const keyParseServerUrl = 'https://parseapi.back4app.com';
  await Parse().initialize(
    keyParseApplicationId,
    keyParseServerUrl,
    clientKey: keyParseClientKey,
    autoSendSessionId: true,
    debug: true,
  );

  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'DataTypes',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  final TextEditingController _objectIdController = TextEditingController();
  final TextEditingController _stringController = TextEditingController();
  final TextEditingController _doubleController = TextEditingController();
  final TextEditingController _jsonController = TextEditingController();
  final TextEditingController _listStringController = TextEditingController();
  final TextEditingController _listIntController = TextEditingController();
  final TextEditingController _listBoolController = TextEditingController();
  final TextEditingController _listJsonController = TextEditingController();
  final TextEditingController _latitudeController = TextEditingController();
  final TextEditingController _longitudeController = TextEditingController();
  final TextEditingController _polygonController = TextEditingController(); // Polygon field
  final TextEditingController _pointerController = TextEditingController(); // Pointer field
  final TextEditingController _relationController = TextEditingController(); // Relation field

  bool _boolValue = false;
  DateTime _selectedDate = DateTime.now();
  String _responseMessage = '';
  PickedFile? pickedFile;
  bool isLoading = false;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('DataTypes'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: SingleChildScrollView(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: <Widget>[
              _buildTextField('Object ID', _objectIdController),
              const SizedBox(height: 20),
              _buildLatitudeLongitudeFields(),
              const SizedBox(height: 20),
              _buildTextField('String Field', _stringController),
              const SizedBox(height: 20),
              _buildTextField('Double Field', _doubleController, isNumeric: true),
              const SizedBox(height: 20),
              _buildSwitch('Bool Field', _boolValue, (value) {
                setState(() {
                  _boolValue = value;
                });
              }),
              const SizedBox(height: 20),
              TextButton(
                onPressed: () => _selectDate(context),
                child: Text('Select Date: ${_selectedDate.toLocal()}'.split(' ')[0]),
              ),
              const SizedBox(height: 20),
              _buildTextField('JSON Field', _jsonController),
              const SizedBox(height: 20),
              _buildTextField('List String Field (comma-separated)', _listStringController),
              const SizedBox(height: 20),
              _buildTextField('List Int Field (comma-separated)', _listIntController),
              const SizedBox(height: 20),
              _buildTextField('List Bool Field (comma-separated)', _listBoolController),
              const SizedBox(height: 20),
              _buildTextField('List JSON Field (pipe-separated)', _listJsonController),
              const SizedBox(height: 20),
              _buildTextField('Polygon Field (JSON)', _polygonController), // Polygon input
              const SizedBox(height: 20),
              _buildTextField('Pointer Field (Object ID)', _pointerController), // Pointer input
              const SizedBox(height: 20),
              _buildTextField('Relation Field (comma-separated Object IDs)', _relationController), // Relation input
              const SizedBox(height: 20),
              _buildImagePicker(), // Image Picker added
              const SizedBox(height: 20),
              ElevatedButton(
                onPressed: _saveData,
                child: const Text('Save'),
              ),
              const SizedBox(height: 10),
              ElevatedButton(
                onPressed: _updateData,
                child: const Text('Update'),
              ),
              const SizedBox(height: 10),
              ElevatedButton(
                onPressed: _deleteData,
                child: const Text('Delete'),
              ),
              const SizedBox(height: 20),
              ElevatedButton(
                onPressed: _fetchData,
                child: const Text('Fetch'),
              ),
              const SizedBox(height: 20),
              Text(_responseMessage),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildTextField(String label, TextEditingController controller, {bool isNumeric = false}) {
    return TextField(
      controller: controller,
      keyboardType: isNumeric ? TextInputType.number : TextInputType.text,
      decoration: InputDecoration(
        border: OutlineInputBorder(),
        labelText: label,
      ),
    );
  }

  Widget _buildLatitudeLongitudeFields() {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: <Widget>[
        _buildTextField('Latitude', _latitudeController, isNumeric: true),
        const SizedBox(height: 20),
        _buildTextField('Longitude', _longitudeController, isNumeric: true),
      ],
    );
  }

  Widget _buildSwitch(String label, bool value, Function(bool) onChanged) {
    return SwitchListTile(
      title: Text(label),
      value: value,
      onChanged: onChanged,
    );
  }

  Widget _buildImagePicker() {
    return GestureDetector(
      child: pickedFile != null
          ? Container(
              width: 250,
              height: 250,
              decoration:
                  BoxDecoration(border: Border.all(color: Colors.blue)),
              child: kIsWeb
                  ? Image.network(pickedFile!.path)
                  : Image.file(File(pickedFile!.path)),
            )
          : Container(
              width: 250,
              height: 250,
              decoration:
                  BoxDecoration(border: Border.all(color: Colors.blue)),
              child: Center(
                child: Text('Click here to pick image from Gallery'),
              ),
            ),
      onTap: () async {
        PickedFile? image = await ImagePicker().getImage(source: ImageSource.gallery);
        if (image != null) {
          setState(() {
            pickedFile = image;
          });
        }
      },
    );
  }

  Future<void> _saveData() async {
    if (pickedFile != null) {
      setState(() {
        isLoading = true;
      });

      ParseFileBase? parseFile;

      if (kIsWeb) {
        parseFile = ParseWebFile(await pickedFile!.readAsBytes(), name: 'file.jpg');
      } else {
        parseFile = ParseFile(File(pickedFile!.path));
      }
      await parseFile.save();

      final gallery = ParseObject('Gallery')
        ..set('file', parseFile)
        ..set('polygon', _parsePolygon(_polygonController.text))
        ..set('pointer', _parsePointer(_pointerController.text))
        ..set('relation', _parseRelation(_relationController.text));

      await gallery.save();

      setState(() {
        isLoading = false;
        pickedFile = null;
      });

      ScaffoldMessenger.of(context)
        ..removeCurrentSnackBar()
        ..showSnackBar(SnackBar(
          content: Text(
            'File saved successfully on Back4app',
            style: TextStyle(color: Colors.white),
          ),
          duration: Duration(seconds: 3),
          backgroundColor: Colors.blue,
        ));
    }
  }

  Future<void> _updateData() async {
    // Update data logic
  }

  Future<void> _deleteData() async {
    // Delete data logic
  }

  Future<void> _fetchData() async {
    // Fetch data logic
  }

  Future<void> _selectDate(BuildContext context) async {
    final DateTime? picked = await showDatePicker(
      context: context,
      initialDate: _selectedDate,
      firstDate: DateTime(2000),
      lastDate: DateTime(2101),
    );
    if (picked != null && picked != _selectedDate)
      setState(() {
        _selectedDate = picked;
      });
  }

  // Convert Polygon field to a suitable format
  ParseObject _parsePolygon(String polygonJson) {
    // Example: {"type": "Polygon", "coordinates": [[[lng, lat], [lng, lat], [lng, lat]]]}
    return ParseObject('Polygon')..set('data', polygonJson);
  }

  // Convert Pointer field to a suitable format
  ParseObject _parsePointer(String objectId) {
    return ParseObject('Pointer')..set('objectId', objectId);
  }

  // Convert Relation field to a suitable format
  List<ParseObject> _parseRelation(String objectIds) {
    final ids = objectIds.split(',').map((id) => ParseObject('Pointer')..set('objectId', id.trim())).toList();
    return ids;
  }
}

```

## This example demonstrates how to:

- Create, update, and delete Parse objects.
- Handle various data types, including strings, doubles, booleans, JSON, lists, and dates.
- Fetch and display Parse objects in a Flutter app.

**Done!**




This code initializes the Parse SDK in Flutter, sets up the main application, and displays a simple home page with a title.

