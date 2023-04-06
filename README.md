# Upgrading Flutter Multipliers

## Errors and Warnings

- Import conflict with `lib/core/presentation/widgets/badge.dart` and `material.dart`. We must rename or use `as` or `show` while importing.
- `SchedulerBinding.instance` no need for null check.
- `Theme.of(context).textTheme.bodyText2` is deprecated. Should use `Theme.of(context).textTheme.bodyMedium` instead.
- `primary` property on TextButton is deprecated and we should use `foregroundColor` instead.

<br>

## Non-breaking package updates:

The following packages can been upgraded to the latest version without any breaking changes:

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
