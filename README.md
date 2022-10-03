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

More to come soon...

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
    Widget build(Buildcontext context) {
        final isPortrait = MediaQuery.of(context).isPortrait // True || False
    }
```

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

