# Dart Gists Library - Dart Extensions

```
 ▄▄▄▄▄▄  ▄▄▄▄▄▄ ▄▄▄▄▄▄   ▄▄▄▄▄▄▄    ▄▄▄▄▄▄▄ ▄▄▄ ▄▄▄▄▄▄▄ ▄▄▄▄▄▄▄ ▄▄▄▄▄▄▄
█      ██      █   ▄  █ █       █  █       █   █       █       █       █
█  ▄    █  ▄   █  █ █ █ █▄     ▄█  █   ▄▄▄▄█   █  ▄▄▄▄▄█▄     ▄█  ▄▄▄▄▄█
█ █ █   █ █▄█  █   █▄▄█▄  █   █    █  █  ▄▄█   █ █▄▄▄▄▄  █   █ █ █▄▄▄▄▄
█ █▄█   █      █    ▄▄  █ █   █    █  █ █  █   █▄▄▄▄▄  █ █   █ █▄▄▄▄▄  █
█       █  ▄   █   █  █ █ █   █    █  █▄▄█ █   █▄▄▄▄▄█ █ █   █  ▄▄▄▄▄█ █
█▄▄▄▄▄▄██▄█ █▄▄█▄▄▄█  █▄█ █▄▄▄█    █▄▄▄▄▄▄▄█▄▄▄█▄▄▄▄▄▄▄█ █▄▄▄█ █▄▄▄▄▄▄▄█
```

A collection of gists and extensions written in the Dart programming language.

More to come soon!!!

---

### 1. Where To Extension

Returns a new type from the matching condition.

#### Extension:

This extension accepts 2 functions, one to conduct a test to decide if it will return this Object into the new Iterable, and one to execute an return a different type of `R`.

This is a synchronous generator function. This will compile into an Iterable of the given type of `T`.

```dart
    extension WhereTo<T> on Iterable<T> {
        Iterable<R> whereTo<R>(bool Function(T element) test, R Function(T element) returnType) sync* {
            for (final item in this) {
                if(test(item)) {
                    yield returnType(item);
                }
            }
        }
    }

```

#### Examples:

```dart

    final list = <int>[1,2,3,4,5];

    final stringIterable = list.whereTo((item) => item == 1 || item == 2, (item) => item.toString());

    print(stringIterable); // ( "1", "2" )

```

```dart

    final names = <String>['Mark', 'Anthony', 'Tim'];

    final personIterable = list.whereTo((name) => name == 'Mark', (name) => Person(name: name));

    print(personIterable); // ( Person(name: 'Mark') )

    final personList = personIterable.toList();

    print(personList);  // [ Person(name: 'Mark') ]

```

---

### 2. Remove All

Removes all keys / values matching the following List of keys.

#### Extension:

```dart
    extension RemoveAll<K, V> on Map<K, V> {
        void removeAll(List<String> keysToRemove) {
            for (final key in keysToRemove) {
                this.remove(key);
            }
        }
    }

```

#### Example:

```dart
    final map = { 'one': 1, 'two': 2, 'three': 3 };

    map.removeAll(['one', 'two']);

    print(map.toString()); // { three: 3 }

```

---

### 3. Is Tablet

Returns a bool of true || false of if the device is a tablet or not.

#### Extension:

`isTablet` checks the `size.shortestSide` property of the MediaQueryData class, then compares it to a breakpoint of 600. If it's greater than this, it is a tablet. If not, it's a high probability of being a mobile device.

```dart
    extension IsTablet on MediaQueryData {
        bool get isTablet => size.shortestSide > 600;
    }
```

#### Example:

Just call this extension in the Stateless or Stateful widget's build method to extact the value.

```dart
    @override
    Widget build(Buildcontext context) {
        final isTablet = MediaQuery.of(context).isTablet // True || False
    }
```

---

### 3. Is Portrait

Returns a bool of true || false of if the device's screen orientation portrait mode or not.

#### Extension:

`isPortrait` checks the MediaQueryData's orientation and cross references it with `Orientation.portrait` enumerable.

`orientation` is a getter of `MediaQueryData`, could also be accessed as `this.orientation`.

```dart
    extension IsPortrait on MediaQueryData {
        bool get isPortrait => orientation == Orientation.portrait;
    }
```

#### Example:

Just call this extension in the Stateless or Stateful widget's build method to extact the value.

```dart
    @override
    Widget build(BuildContext context) {
        final isPortrait = MediaQuery.of(context).isPortrait // True || False
    }
```

---

### 4. Log Object

Logs any Object as a String in the debug console.

#### Extension:

```dart

    import 'dart:developer' as devtools;

    extension LogObject on Object {
        log() => devtools.log(this.toString());
    }
```

#### Example:

```dart
    final map = {'test': [1,2,3,4]};

    map.log(); // {test: [1,2,3,4]};
```

---

### 5. Print Map

Logs your map's key's and values to the debug console in an easy to read format.

#### Extension:

```dart
    import 'dart:developer' as devtools;

    extension PrintMap<K, V> on Map<K, V> {
        void printMap() {
            devtools.log('{');
            for(final entry in entries) {
                devtools.log('\n\t${entry.key} : ${entry.value}');
            }
            devtools.log('}');
        }
    }
```

#### Example:

```dart
    final map = {'name': 'Bill', 'age': 21};

    map.printMap();

    // {
    //    name : Bill
    //    age: 21
    // }

```

---

### 6. Remove Empty String

Remove's all empty strings from any `List<String>`

#### Extension:

```dart
    extension RemoveEmptyString on List<String> {
        List<String> removeEmptyString() {
            final list = this;
            list.removeWhere((item) => item.isEmpty);
            return this;
        }
    }
```

#### Example:

```dart
    final list = ['1', '', '2', '3', ''];

    final newList = list.removeEmptyString(); // ['1', '2', '3']
```

---

### 6. Detect Swipe Direction on Drag Details

Detects the direction of the swipe and makes utilizes a callback based on direction.

#### Extension:

```dart
    extension DetectSwipeDirectionAndCall on DragEndDetails {
        void detectWipeDirectionAndCall(Function(void l)? left, Function(void r)? right) {
            final direction = primaryVelocity ?? 0.0;

            if(direction > 0) {
                left?.call(null);
            }

            if(direction < 0) {
                right?.call(null);
            }
        }
    }
```

#### Example:

```dart
    GestureDetector(
        onHorizontalDragEnd: (details) {
            details.detectSwipeDirectionAndCall(
                (l) {
                    print('Swiped left!');
                },
                (r) {
                    print('Swiped right!');
                },
            )
        }
    )
```

---

### 7. Capitalize the First Letter of a String

Capitalizes the first letter of a String.

#### Extension:

```dart

    extension CapitalizeFirstLetter on String {
        String firstLetter() {
            return '${this[0].toUpperCase()}${substring(1)}';
        }
    }

```

#### Example:

```dart
    final myString = 'battleship';

    print(myString.firstLetter()); // 'Battleship'
```

---

### 7. Find the Index of an Object on an Iterable

Loops through the Iterable and finds the index of a given Object.

#### Extension:

```dart

    int indexOf<T>(T object) {
        for (int i = 0; i < length; i++) {
            final element = elementAt(i);
            if (object == element) {
                return i;
            }
        }
        return -1;
    }

```

#### Example:

```dart
    final Iterable myIterable = data['iterable']; // ('hello', 'apple', 'tree')

    print(myIterable.indexOf('apple')); // 0
```

---

### 8. Format any DateTime to String

Formats any DateTime class into a String using the DateFormat class.

#### Extension:

```dart

    import 'package:intl/intl.dart';

    extension FormatDateToString on DateTime {
        String formatDateToString(String format) => DateFormat(format).format(this);
    }

```

#### Example:

```dart

    final date = DateTime.now();

    final dateString = date.formatDateToString.('MMMM dd, yyyy'); // February 2nd, 2023

```

---

### 9. Convert DateTime's dayOfWeek to String

Formats the `int` given from DateTime's dayOfWeek property into a `String` representing the day of the week.

#### Extension:

```dart
    extension ConvertDayOfWeekToString on int {
        String get convertDayOfWeekToString {
            final shadow = this;

            switch (shadow) {
                case DateTime.monday:
                    return 'Monday';
                case DateTime.tuesday:
                    return 'Tuesday';
                case DateTime.wednesday:
                    return 'Wednesday';
                case DateTime.thursday:
                    return 'Thursday';
                case DateTime.friday:
                    return 'Friday';
                case DateTime.saturday:
                    return 'Saturday';
                case DateTime.sunday:
                    return 'Sunday';
                default:
                    return 'Unknown Date';
            }
        }
    }

```

#### Example:

```dart

    final dayOfWeek = DateTime.now().dayOfWeek; // 3

    final nameOfDay = dayOfWeek.convertDayOfWeekToString;  // Wednesday

```

---

### 10. Find All Dates of a Week

Given a week number `int` out of a possible 52 - 53 weeks of the year, this will return an [Iterable] of [DateTime] for each day of the week specified. Starts on Monday (first day) and ends on Sunday (last day).

#### Extension:

```dart
    extension FindAllDatesOfWeekNumber on int? {
        Iterable<DateTime> get findAllDatesOfWeekNumber {
            final shadow = this;
            if(shadow == null) return [];

            // The last day of the week
            // If shadow = 2 then Jan 14th
            final lastDay = shadow * 7;

            final currentYear = DateTime.now().year // Ex: 2023

            // Starts at the first day of the year and adds the number of days to target the last day of the week
            final lastWeekOfDay = DateTime.utc(currentYear, 1, 1).add(Duration(days: lastDay));

            final days = List.generate(7, index) {
                return lastWeekDay.subtract(Duration(days: index));
            }).reversed;

            return days;
        }
    }

```

#### Example:

```dart

    final weekNumber = 23;
    final datesOfWeekList = weekNumber.findAllDatesOfWeekNumber;

    // (2023-02-06 00:00:00.000Z, ... 2023-02-12 00:00:00.000Z)

```

---

### 11. Find All Week Numbers of a Year

Returns an [Iterable] of [int] for every week number of a given year.
The year is specified as an [int], such as the year 2023.

#### Extension:

```dart
    extension WeekNumbersList on int? {
        Iterable<int> get weekNumbersList {
            final currentYear = this;
            if (currentYear == null) return [];

            final numberOfWeeksForYear = (DateTime.utc(currentYear, 12, 31).difference(DateTime.utc(currentYear, 1, 1)).inDays / 7).ceil();

            return Iterable.generate(numberOfWeeksForYear, (index) => index + 1);
        }
    }

```

#### Example:

```dart

    final year = DateTime.now().year;
    final weeksInYear = year.weekNumbersList; // (1, 2, 3, ..., 51, 52)

```
