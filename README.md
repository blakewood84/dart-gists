# Dart Gists Library

### 1. Where To Extension

Returns a new type from the matching condition.

Example:

```dart

    final list = <int>[1,2,3,4,5];
    
    final stringList = list.whereTo((item) => item == 1 || item == 2, (item) => item.toString());

    print(stringList); // ( "1", "2" )

```

<p align="center"><img src="/where_to/where_to.png" alt="Where To Extension"/></p>

### 2. Remove All

Removes all keys / values matching the following List of keys.

Example: 

```dart
    final map = { 'one': 1, 'two': 2, 'three': 3 };

    map.removeAll(['one', 'two']);

    print(map.toString()); // { three: 3 } 

```

<p align="center"><img src="/remove_all/remove_all.png" alt="Remove All Extension"/></p>