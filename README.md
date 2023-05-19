# Upgrading Flutter Multipliers

This document contains the list of changes that need to be made to upgrade the `flutter-multipliers` project to the `3.10.1` version of Flutter.

We also prepared a list of articles/resources that may be helpful to learn about new amazing things in Dart 3.0 that comes with this upgrade [here](dart_3_things.md).

## Errors and Warnings

- Import conflict with `lib/core/presentation/widgets/badge.dart` and `material.dart`. We must use `as` prefix to avoid this conflict.
- `SchedulerBinding.instance` no need for null check.
- `Theme.of(context).textTheme.bodyText2` is deprecated. Should use `Theme.of(context).textTheme.bodyMedium` instead.
- `primary` property on TextButton is deprecated and we should use `foregroundColor` instead.
- `primary` property on ElevatedButton is deprecated and we should use `backgroundColor` instead.
- `Bool` parameters must be named and not positional. This is for better readability.
-  We need to use `const` for widgets that are not dynamic for performance improvements.
- `window` is deprecated for flutter's upcoming multi-window support. Following changes need to be made: 

    ```dart

    // Before
    import 'dart:ui' as ui;

    MediaQueryData.fromWindow(ui.window);

    // After
    import 'package:flutter/material.dart';

    MediaQueryData.fromWindow(View.of(context));

    ```
- `channel.setMockMethodCallHandler` is deprecated and shouldn't be used: Replace it with `TestDefaultBinaryMessengerBinding.instance.defaultBinaryMessenger.setMockMethodCallHandler`

    ```
    //Before:
    setUp(() {
        channel.setMockMethodCallHandler((MethodCall methodCall) async {
          log.add(methodCall);
      return true;
       });
     });

    // after
    setUp(() {
         TestDefaultBinaryMessengerBinding.instance.defaultBinaryMessenger
      .setMockMethodCallHandler(channel, (MethodCall methodCall) async {
        log.add(methodCall);
      return true;
      });
  });
    ```

## Analysis Options

- `implicit-dynamic` is removed.
- `prefer_equal_for_default_values` is removed.


<br>

## Non-breaking package updates:

The following packages can be upgraded to the latest version without any breaking changes:

- **lottie**: 2.3.0
- **marquee**: 2.2.3
- **sqflite**: 2.2.6
- **shared_preferences**: 2.0.18
- **equatable**: 2.0.5
- **flutter_secure_storage**: 8.0.0
- **cached_network_image**: 3.2.1

<br>

## Breaking packages updates

- [flutter_bloc](flutter_bloc.md)
- [flutter_svg](flutter_svg.md)
- [flutter_local_notifications](flutter_local_notifications.md)
- [adjust_sdk](adjust_sdk.md)
- [firebase packages](firebase.md)
- [moor](moor.md)


