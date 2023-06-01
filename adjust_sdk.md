# Changes in `adjust_sdk` package

## New version number:

- adjust_sdk: 4.33.1

---

## Requirements:

N/A

---

## Resolving issues:

Before upgrading `adjust_sdk`, the following used to be logged in the console for android as `getAppTrackingAuthorizationStatus` is not supported in android.

``` dart
E/AdjustBridge( 7388): Not implemented method: getAppTrackingAuthorizationStatus
```

Upon upgrading the package, we get the following error:

``` dart
[log] _TypeError (type 'String' is not a subtype of type 'int')
[log] #0      Adjust.getAppTrackingAuthorizationStatus
adjust.dart:107
        <asynchronous suspension>
#1      _AppState._initializeAdjust
        <asynchronous suspension>
#2      _AppState._initialize
app.dart:200
        <asynchronous suspension>
```

We can solve this by catching the TypeError on \_initializeAdjust method in `app.dart` `line 288` as follows:

``` dart
Future<void> _initializeAdjust() async {
    final AdjustConfig configuration =
        AdjustConfig(adjustDerivGoToken, AdjustEnvironment.production)
          ..logLevel = AdjustLogLevel.verbose;

    // Show the iOS IDFA dialog If the app tracking transparency is in notDetermined state.
    try {
      if (await Adjust.getAppTrackingAuthorizationStatus() == 0) {
        final num status =
            await Adjust.requestTrackingAuthorizationWithCompletionHandler();

        // If app tracking is allowed by the user we set IDFA reading to true.
        configuration.allowIdfaReading = status == 3;
      }
    } on MissingPluginException {
      dev.log('AppTrackingAuthorization is not available');
    } on TypeError { // ADD THIS LINE
      dev.log('AppTrackingAuthorization is not supported for android.');
    }

    Adjust.start(configuration);
  }
```
