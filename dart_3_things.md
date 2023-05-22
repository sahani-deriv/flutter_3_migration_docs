# New things in Dart 3 

Following are the new important/interesting things introduced starting from Dart 3.0.

## Records

New built-in type in Dart 3 which is immutable. Like any other collection types, it lets you bundle multiple objects in a single opject. 

```dart

var record = (1, a: "2", b: 3);

record.$1; // 1
record.a; // "2"
```

When is it useful? Mostly, when you want multiple return types.

<b>Example:</b>

Suppose we have a json response from an API call:

```dart
{
  "name": "John Doe",
  "age": 30,
}
```
Previously we would either use a `List<Object>` or create a class with `name` and `age` to get both the values, which of course had some drawbacks.

We would do:

```dart
List<Object> getUserInfo(Map<String, dynamic> json){
    return [json['name'], json['age']];
}
```

OR

```dart
class UserInfo{
    final String name;
    final int age;

    UserInfo({required this.name, required this.age});
}

UserInfo getUserInfo(Map<String, dynamic> json){
    return UserInfo(name: json['name'], age: json['age']);
}
```

The first approach is not ideal as it relies on the position of values in a list, which can be error-prone and hard to maintain. Similarly, the second approach of creating a separate class for just two values can be overly verbose and unnecessary.With the introduction of `records` in Dart 3, a more concise and safer solution is available.

We can used `named records` to get the same result as above:

```dart

({String name, int age}) userInfo(Map<String, dynamic> json) {
  return (name: json['name'] as String, age: json['age'] as int);
}

print(userInfo(json).name); // John Doe
```

Other use case might be controlling flow of arguments: 
- https://mokshmahajan.hashnode.dev/streamlining-flutter-code-with-records-and-control-flow

Find more at: - https://dart.dev/language/records

## Pattern Matching and Code Destructuring

Imagine having a switch case like below: 

```dart
 final a = 40;

  switch(a){
    case >=10 && <=50:
      print('a is greater than 10');
      break;
  }

```
Or being able to do this: 

```dart
var numList = [1, 2, 3];
var [a, b, c] = numList;

print(a); // 1
```

Or this: 

```dart
(lat, long) getLatLong() => (40.7128, 74.0060);
print(getLatLong().lat); // 40.7128
```

Well, you can do it now with Dart 3.0 and reading the links below will show you much more you can do now.

- https://dart.dev/language/patterns
- https://mokshmahajan.hashnode.dev/unlock-the-magic-of-pattern-matching-in-dart-30
- https://medium.com/@dnkibere/dart-3-records-pattern-matching-sealed-classes-and-more-12a9e3a52447
- https://www.youtube.com/watch?v=KhYTFglbF2k

You can get more used it by doing the exercise below: 
https://codelabs.developers.google.com/codelabs/dart-patterns-records#0
## Class Modifiers

Dart 3.0 introduces us to `Fiveeee` new class modifiers to allow an author to control whether the type allows being implemented, extended, and/or mixed in from outside of the library where it's defined. Check the links below to find out about new modifiers.

- https://dart.dev/language/class-modifiers
- https://medium.com/lost-but-coding/yo-flutter-devs-i-heard-you-like-class-modifiers-135a8406f303

To understand more about the `WHY/WHEN_TO USE` about the new class modifiers: 
- https://github.com/dart-lang/language/blob/main/accepted/future-releases/class-modifiers/feature-specification.md

## Enhanced Enums (Was in Dart 2.17 but wanted to include it here)

Enums are now more powerful than ever. You can now add methods, properties, and constructors to enums. 

- https://medium.com/huawei-developers/flutter-enhanced-enums-the-powerful-feature-of-dart-2-17-new-feature-of-flutter-3-0-8177a926e487

