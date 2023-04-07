# Changes in `flutter_local_notifications` package

## New version number:

- flutter_local_notifications: 13.0.0

---

## Requirements:

N/A

---

Resolving issues:

- The following snippet is from `fcm_helper.dart` starting `line 42`.

  ```dart
  Future<void> configureNotificationSettings({
    required SelectNotificationCallback onSelectNotificationCallback,
  }) async {
    const AndroidInitializationSettings android =
        AndroidInitializationSettings('ic_notification');

    const IOSInitializationSettings iOS = IOSInitializationSettings();

    const InitializationSettings initializationSettings =
        InitializationSettings(android: android, iOS: iOS);

    await _notificationPlugin.initialize(initializationSettings,
        onSelectNotification: onSelectNotificationCallback);
  }
  ```

- Upon upgrading the `flutter_local_notifications` package, `SelectNotificationCallback` and `IOSInitializationSettings` is no longer valid type and `onSelectNotification` is no longer a valid parameter which caused compile time error. To solve this we will need to change the snippet to the following:

  ```dart
  Future<void> configureNotificationSettings({
    required DidReceiveNotificationResponseCallback
        onDidReceiveNotificationResponse,
  }) async {
    const AndroidInitializationSettings android =
        AndroidInitializationSettings('ic_notification');

    const DarwinInitializationSettings iOS = DarwinInitializationSettings();

    const InitializationSettings initializationSettings =
        InitializationSettings(android: android, iOS: iOS);

    await _notificationPlugin.initialize(initializationSettings,
        onDidReceiveNotificationResponse: onDidReceiveNotificationResponse);
  }
  ```

- We also need to change `Line 75`

  ```dart
  const IOSNotificationDetails iOS = IOSNotificationDetails(
    presentAlert: true,
    presentBadge: true,
    presentSound: true,
  );
  ```

  to

  ```dart
  const DarwinNotificationDetails iOS = DarwinNotificationDetails(
    presentAlert: true,
    presentBadge: true,
    presentSound: true,
  );
  ```
- On making the above changes, we will get another issue in `fcm_helper.dart` on `line 71` as it is using the method `configureNotificationSettings` in the following:

  ```dart
  await configureNotificationSettings(
        onSelectNotificationCallback: (String? payload) => onNotificationSelected(
          payload: payload,
          secureStorage: widget._secureStorage,
        ),
      );
  ```

- As we renamed `onSelectNotificationCallback` to `onDidReceiveNotificationResponse`, and `DidReceiveNotificationResponseCallback` expects `void Function(NotificationResponse)` and not `void Function(String?)`. We will need to change the above code to the following:

  ```dart
  await configureNotificationSettings(
        onDidReceiveNotificationResponse:
            (NotificationResponse notificationResponse) => onNotificationSelected(
          payload: notificationResponse.payload,
          secureStorage: widget._secureStorage,
        ),
      );
  ```
