original: https://github.com/couchbaselabs/mobile-dart-flutter-plugin

target: Android, iOS, Web

<br>

### 実行環境 (Apr 17, 2025)
Flutter 3.24以降ではNG → https://github.com/cbl-dart/cbl-dart/issues/598
```
$ flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[!] Flutter (Channel [user-branch], 3.22.3, on macOS 14.7.1 23H222 darwin-x64, locale en-JP)
    ! Flutter version 3.22.3 on channel [user-branch] at /usr/local/Caskroom/flutter/3.29.2/flutter
      Currently on an unknown channel. Run `flutter channel` to switch to an official channel.
      If that doesn't fix the issue, reinstall Flutter by following instructions at https://flutter.dev/docs/get-started/install.
    ! Upstream repository unknown source is not a standard remote.
      Set environment variable "FLUTTER_GIT_URL" to unknown source to dismiss this error.
[✓] Android toolchain - develop for Android devices (Android SDK version 36.0.0)
[✓] Xcode - develop for iOS and macOS (Xcode 16.2)
[✓] Chrome - develop for the web
[✓] Android Studio (version 2024.3)
[✓] VS Code (version 1.99.2)
[✓] Connected device (6 available)
[✓] Network resources

! Doctor found issues in 1 category.
```

<br>

### example 動作確認結果 (Apr 17, 2025)

チャット画面の起動と `Send` ボタンが押せるまで。機能確認にはサーバ (Sync Gateway) が必要

✅ Chorme 135.0.7049.85 - Ok

✅ iOS 15.8.4, 18.2.1 - Ok

🆖 android-arm64  • Android 10 (API 29) - Failed
```
$ flutter run -d ...
FAILURE: Build failed with an exception.

* What went wrong:
Could not open cp_settings generic class cache for settings file '/Users/user/Desktop/mobile-dart-flutter-plugin/example/android/settings.gradle' (/Users/user/.gradle/caches/7.5/scripts/5bahl4yp5mw7lcn2efkltdtgf).
> BUG! exception in phase 'semantic analysis' in source unit '_BuildScript_' Unsupported class file major version 65

* Try:
> Run with --stacktrace option to get the stack trace.
> Run with --info or --debug option to get more log output.
> Run with --scan to get full insights.

* Get more help at https://help.gradle.org

BUILD FAILED in 910ms
Running Gradle task 'assembleDebug'...                           1,422ms

┌─ Flutter Fix ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ [!] Your project's Gradle version is incompatible with the Java version that Flutter is using for Gradle.                               │
│                                                                                                                                         │
│ If you recently upgraded Android Studio, consult the migration guide at docs.flutter.dev/go/android-java-gradle-error.                  │
│                                                                                                                                         │
│ Otherwise, to fix this issue, first, check the Java version used by Flutter by running `flutter doctor --verbose`.                      │
│                                                                                                                                         │
│ Then, update the Gradle version specified in                                                                                            │
│ /Users/user/Desktop/mobile-dart-flutter-plugin/example/android/gradle/wrapper/gradle-wrapper.properties to be compatible with that Java │
│ version. See the link below for more information on compatible Java/Gradle versions:                                                    │
│ https://docs.gradle.org/current/userguide/compatibility.html#java                                                                       │
│                                                                                                                                         │
│                                                                                                                                         │
└─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
Error: Gradle task assembleDebug failed with exit code 1
```

<br>

以下、オリジナルのREADME

---

# Couchbase Lite Flutter Plugin

This plugin provides Flutter support for Couchbase Lite, enabling developers to integrate powerful mobile database capabilities and web into their Flutter applications[2].

## Overview

Couchbase Lite Flutter Plugin allows you to leverage the full potential of Couchbase Lite in your Flutter projects. It offers a seamless integration between Flutter and Couchbase Lite, providing a robust solution for offline-first mobile application and web support.

## Installation

To use this plugin in your Flutter project, add the following dependency to your `pubspec.yaml` file:

```yaml
dependencies:
  couchbase_lite_flutter: ^latest_version
```

Replace `latest_version` with the most recent version of the plugin[2].

## Usage

Here's a basic example of how to use the Couchbase Lite Flutter Plugin in your application:

## Code Example

```dart
import 'package:cbl/cbl.dart';

Future<void> run() async {
  // Open the database (creating it if it doesn’t exist).
  final database = await Database.openAsync('chat_database');

  // Create a collection for chat messages, or return it if it already exists.
  final collection = await database.createCollection('messages');

  // Create a new chat message document.
  final mutableDocument = MutableDocument({
    'sender': 'Alice',
    'recipient': 'Bob',
    'message': 'Hello, Bob!',
    'timestamp': DateTime.now().toIso8601String(),
  });
  await collection.saveDocument(mutableDocument);

  print(
    'Created message with id ${mutableDocument.id} from '
    '${mutableDocument.string('sender')} to ${mutableDocument.string('recipient')}.',
  );

  // Update the message.
  mutableDocument.setString('Hello, Bob! How are you?', key: 'message');
  await collection.saveDocument(mutableDocument);

  print(
    'Updated message with id ${mutableDocument.id}, '
    'new message: ${mutableDocument.string("message")!}.',
  );

  // Read the message document.
  final document = (await collection.document(mutableDocument.id))!;

  print(
    'Read message with id ${document.id}, '
    'from ${document.string('sender')} to ${document.string('recipient')} with '
    'content: ${document.string('message')}.',
  );

  // Create a query to fetch all messages sent by Alice.
  print('Querying messages sent by Alice.');
  final query = await database.createQuery('''
    SELECT * FROM messages
    WHERE sender = 'Alice'
  ''');

  // Run the query.
  final result = await query.execute();
  final results = await result.allResults();
  print('Number of messages sent by Alice: ${results.length}');

  // Close the database.
  await database.close();
}
```

## Features

- **Full Couchbase Lite API**: Access to all Couchbase Lite features including CRUD operations, queries, and replication.
- **Cross-platform**: Works on both iOS, Android and Web platforms.
- **Offline-first**: Build mobile apps that work offline and sync when a connection is available.
- **Performance**: Utilizes native Couchbase Lite libraries for optimal performance.

## Documentation

For detailed documentation and API reference, please visit our [official documentation page](https://github.com/couchbaselabs/mobile-dart-flutter-plugin/wiki/Documentation).

## Contributing

We welcome contributions to the Couchbase Lite Flutter Plugin!.

## License

This project is licensed under the Apache License 2.0.

## Support

For questions, feature requests, or bug reports, please file an issue on our [GitHub issues page](https://github.com/couchbaselabs/mobile-dart-flutter-plugin/issues).
