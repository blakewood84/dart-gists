# Dart Gists Library - Dart Extensions

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

Examples:

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

Example: 

```dart
    final map = { 'one': 1, 'two': 2, 'three': 3 };

    map.removeAll(['one', 'two']);

    print(map.toString()); // { three: 3 } 

```

<p align="center"><img src="/remove_all/remove_all.png" alt="Remove All Extension"/></p>

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