# Changes in `flutter_svg` package

## New version number:

- flutter_svg: 2.0.0

---

## Requirements:

N/A

---

## Resolving issues:

- `color` has been deprecated in `SvgPicture.asset(...)`. We will need to replace it with `colorFilter`.

- The test case in `test/features/notification/system_notification/notification_item_test` fails due to the following issues:

  - The getter `pictureProvider` isn't defined for the type `SvgPicture` anymore.
  - `ExactAssetPicture` isn't defined anymore.

    Code snippet reference:

    ``` dart
    WidgetPredicate _widgetPredicate(S localization, String icon) =>
        (Widget widget) =>
            widget is SvgPicture &&
            widget.semanticsLabel ==
                localization.semanticNotificationInformationIcon &&
            (widget.pictureProvider is sn &&
                (widget.pictureProvider as ExactAssetPicture).assetName ==
                    notificationWarningIcon);
    ```

- As per the documentation of `flutter_svg`, `pictureProvider` no longer provides any kind of functionality and `ExactAssetPicture` belonged to `pictureProvider` itself. We can safely remove the line where we used it.
