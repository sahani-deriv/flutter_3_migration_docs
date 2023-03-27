# Changes in `flutter_bloc` package

## New version number:

- ## flutter_bloc: 8.1.2
<!-- <br> -->

---

## Requirements:

Upgrading to v8.1.2 in `flutter-multipliers` will require us to upgrade the `flutter_bloc` and `bloc` in following packages:

**Non-breaking bloc migrations**:

- `deriv_auth`, `deriv_web_view`, `deriv_bloc_manager`.

**Breaking bloc migrations**:

- `flutter_deriv_api`, `update_checker`.
- `mapEventToState` has been removed since bloc 8.0.0. We will need to change `mapEventToState` to `onEvent` in these packages.

-We also have to upgrade `bloc_test` to v9.1.1, wherever we have upgraded `flutter_bloc`.

## <!-- <br> -->

## Resolving issues:

- Since `flutter_bloc` v8.0.0, `BlocUnhandledErrorException` has been removed, which results in following error as we have used it in our test cases.

  ```
  The name 'BlocUnhandledErrorException' isn't a type, so it can't be used as a type argument.
  Try correcting the name to an existing type, or defining a type named 'BlocUnhandledErrorException'.
  ```

  We have used this exception in `test/features/chart/state/chart_setting/chart_setting_cubit_test.dart`.

  ```
      test(
          'should emit a [$ChartSettingFailureState] when the service throws '
          'an exception',
          () async {
          when(service.getSetting).thenThrow(Exception('Test exception'));

          expect(
              chartSettingCubit.loadLastSetting,
              throwsA(isA<BlocUnhandledErrorException>()),
          );
          expect(
              chartSettingCubit.state,
              isA<ChartSettingFailureState>(),
          );
          },
      );

  ```

  We came to the conclusion to remove the assertion for `BlocUnhandledErrorException` and not add any replacements as well due to the reason below:

  ```
  /// Loads last saved chart setting from shared preferences.
  Future<void> loadLastSetting() async {
      try {
      final ChartSettingModel setting = await _service.getSetting();
      emit(ChartSettingLoadedState(setting));
      } on Exception catch (error, stackTrace) {
      emit(ChartSettingFailureState(message: '$error', setting: state.setting));
      addError(error, stackTrace);
      }
  }
  ```

  The above snippet is from the method we are testing. We can see that we are catching the exception and calling `bloc.addError` method which reports error to `bloc.onError`. As per **changelog of bloc v8.0.0**, only uncaught exceptions are rethrown by `bloc.onError`. Since, it is a caught exception, it will not be rethrown and only log the error to the console as we have added log in our `BlocObserver`. So, there will not be any exceptions thrown to test. We can change the test case to the following:

  ```
  test(
          'should emit a [$ChartSettingFailureState] when the service throws '
          'an exception',
          () async {
          when(service.getSetting).thenThrow(Exception('Test exception'));
          await chartSettingCubit.loadLastSetting();
          expect(
              chartSettingCubit.state,
              isA<ChartSettingFailureState>(),
          );
          },
      );
  ```

  - Warning: The member 'addError' can only be used within instance members of subclasses of 'package:bloc/src/bloc_base.dart'. This is received in the following code snippet.

  ```
  blocTest<TickStreamCubit, TickStreamState>(
          'captures exceptions.',
          build: () => TickStreamCubit(),
          act: (TickStreamCubit cubit) => cubit.addError(exception),
          errors: () => <Matcher>[equals(exception)],
      );
  ```

  The above lint error/warning is due to `bloc.addError` being protected in the new version. Upon checking the official git repository of `bloc` to see how the addError is being tested, we found that they have added `//ignore: invalid_use_of_protected_member` in the file to remove the lint error. We can do the same in our test cases as we haven't found the alternative way to test the case without having to use the protected `addError`.
