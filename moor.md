# Changes in `moor` package

***`Moor` package has been renamed to `Drift`. The reason for this is that, in some parts of the world, `moor` may be used as a derogatory term.Learn more about migration***   [here][moor_migration_link].

## Dependencies Changes:
Following additions and replacements should be done in the `pubspec.yaml` of `flutter-multipliers`.
### Replacements:
- moor_flutter: ^4.0.0 --> drift_sqflite: ^2.0.0
- moor_generator: ^4.6.0 --> drift_dev: ^2.6.0

### Additions:
- drift: ^2.6.0

<!-- <br> -->

---

## Changes:


- `lib/core/db/moor/moor_helper.dart` 
    **Imports**:

    ```dart
    import 'package:flutter_multipliers/features/notification/enums.dart';
    import 'package:moor_flutter/moor_flutter.dart';
    ```

    should be changed to:
        
    ```dart
    import 'package:drift/drift.dart';
    import 'package:flutter_multipliers/features/notification/enums.dart';
    import 'package:drift_sqflite/drift_sqflite.dart';
    ```

    **Line 8**
    ```dart
    @UseMoor(tables: <Type>[NewsNotifications, SystemNotifications])
    ```

    should be changed to:

    ```dart
    @DriftDatabase(tables: <Type>[NewsNotifications, SystemNotifications])
    ```

    **Line 10 - Line 11**
    ```dart
    AppDatabase._internal()
      : super(FlutterQueryExecutor.inDatabaseFolder(path: 'deriv.sqlite'));
    ```

    should be changed to:

    ```dart
    AppDatabase._internal()
      : super(SqfliteQueryExecutor.inDatabaseFolder(path: 'deriv.sqlite'));
    ```

- `lib/features/notification/helpers/db_notification_helper.dart` Line 1:

    ```dart
    import 'package:moor_flutter/moor_flutter.dart';
    ```

    should be changed to:

    ```dart
    import 'package:drift/drift.dart';
    ```

- `lib/core/db/moor/moor_tables.dart` 

    ```dart
    // ignore_for_file: always_specify_types

    part of 'moor_helper.dart';

    /// Schema of NewsNotifications table in the local db.
    class NewsNotifications extends Table {
    /// Primary key of the table.
    Column<int?> get id => integer().autoIncrement()();

    /// The web url  of the news.
    Column<String?> get webUrl => text()();

    /// Title of the news.
    Column<String?> get title => text()();

    /// Description of the news.
    Column<String?> get description => text()();

    /// Image url of the news.
    Column<String?> get imageUrl => text().nullable()();

    /// Boolean value that decides where the news url should open.
    /// Opens in `InAppBrowser` if `true` otherwise opens in default browser.
    Column<bool?> get isInAppBrowser =>
        boolean().withDefault(const Constant<bool>(true))();

    /// Boolean value that represents whether the news is viewed or not.
    Column<bool?> get isViewed =>
        boolean().withDefault(const Constant<bool>(false))();
    }

    /// Schema of SystemNotifications table in the local db.

    class SystemNotifications extends Table {
    ///  Primary key and notification id of the table
    Column<String?> get notificationId => text().nullable()();

    /// Title of the system Notification
    Column<String?> get notificationTitle => text()();

    /// Description of the system Notification.
    Column<String?> get notificationDescription => text()();

    /// Boolean value that represents whether the notification is read or not.
    Column<bool?> get notificationRead =>
        boolean().withDefault(const Constant<bool>(false)).nullable()();

    /// Boolean value that represents whether the notification is viewed or not.
    Column<bool?> get notificationViewed =>
        boolean().withDefault(const Constant<bool>(false)).nullable()();

    /// account id of the system Notification.
    Column<String?> get acct => text().nullable()();

    /// action status of the system Notification.
    Column get notificationAction => intEnum<NotificationAction>().nullable()();

    /// type status of the system Notification.
    Column get notificationType => intEnum<NotificationType>().nullable()();

    @override
    Set<Column> get primaryKey => {notificationId};
    }
    ```

    should be changed to:

    ```dart
    // ignore_for_file: always_specify_types

    part of 'moor_helper.dart';

    /// Schema of NewsNotifications table in the local db.
    class NewsNotifications extends Table {
    /// Primary key of the table.
    IntColumn get id => integer().autoIncrement()();

    /// The web url  of the news.
    TextColumn get webUrl => text()();

    /// Title of the news.
    TextColumn get title => text()();

    /// Description of the news.
    TextColumn get description => text()();

    /// Image url of the news.
    TextColumn get imageUrl => text().nullable()();

    /// Boolean value that decides where the news url should open.
    /// Opens in `InAppBrowser` if `true` otherwise opens in default browser.
    BoolColumn get isInAppBrowser =>
        boolean().withDefault(const Constant<bool>(true))();

    /// Boolean value that represents whether the news is viewed or not.
    BoolColumn get isViewed =>
        boolean().withDefault(const Constant<bool>(false))();
    }

    /// Schema of SystemNotifications table in the local db.

    class SystemNotifications extends Table {
    ///  Primary key and notification id of the table
    TextColumn get notificationId => text().nullable()();

    /// Title of the system Notification
    TextColumn get notificationTitle => text()();

    /// Description of the system Notification.
    TextColumn get notificationDescription => text()();

    /// Boolean value that represents whether the notification is read or not.
    BoolColumn get notificationRead =>
        boolean().withDefault(const Constant<bool>(false)).nullable()();

    /// Boolean value that represents whether the notification is viewed or not.
    BoolColumn get notificationViewed =>
        boolean().withDefault(const Constant<bool>(false)).nullable()();

    /// account id of the system Notification.
    TextColumn get acct => text().nullable()();

    /// action status of the system Notification.
    Column get notificationAction => intEnum<NotificationAction>().nullable()();

    /// type status of the system Notification.
    Column get notificationType => intEnum<NotificationType>().nullable()();

    @override
    Set<Column> get primaryKey => {notificationId};
    }

    ```

**After the above changes, you can now run `flutter pub run build_runner build` to generate the code. We might run into some issues. To fix it refer to the following:** 

- `test/features/notification/presentation/widgets/system_notification/notification_item_test`
    **Import**

    ```dart
    import 'package:drift/drift.dart';
    ```



    **Line 76 - Line 77**

    ```dart
    notificationsModel.systemNotifications.first
          .copyWith(notificationType: NotificationType.info);
    ```
    should be changed to:

    ```dart
    notificationsModel.systemNotifications.first.copyWith(
          notificationType:
              const Value<NotificationType>(NotificationType.info));
    ```

    **Line 92 - Line 93**

    ```dart
    notificationsModel.systemNotifications.first
          .copyWith(notificationType: NotificationType.danger);
    ```

    should be changed to:

    ```dart
    notificationsModel.systemNotifications.first.copyWith(
          notificationType:
              const Value<NotificationType>(NotificationType.danger));
    ```




[moor_migration_link]: https://drift.simonbinder.eu/docs/upgrading/

