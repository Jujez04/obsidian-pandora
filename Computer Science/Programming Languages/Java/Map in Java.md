`Map` is an interface in Java that creates a function that maps certain type of object to another type of object. They could belong to the same class, but we can map different type of object, including data structures. In the UML map we can notice that extends `Collection` and there are two different implementations, one that uses `hashCode` and on the other hand builds a red-black tree.

![[Pasted image 20241026212925.png]]

Looking in the interface we can see that there is an association between a *value* and a *key* that stores a *value* inside the map.

| Method | Action Performed |
|--------|-------------------|
| `clear()` | This method is used in Java Map Interface to clear and remove all of the elements or mappings from a specified Map collection. |
| `containsKey(Object)` | This method is used in Map Interface in Java to check whether a particular key is being mapped into the Map or not. It takes the key element as a parameter and returns True if that element is mapped in the map. |
| `containsValue(Object)` | This method is used in Map Interface to check whether a particular value is being mapped by a single or more than one key in the Map. It takes the value as a parameter and returns True if that value is mapped by any of the keys in the map. |
| `entrySet()` | This method is used in Map Interface in Java to create a set out of the same elements contained in the map. It basically returns a set view of the map or we can create a new set and store the map elements into them. |
| `equals(Object)` | This method is used in Java Map Interface to check for equality between two maps. It verifies whether the elements of one map passed as a parameter is equal to the elements of this map or not. |
| `get(Object)` | This method is used to retrieve or fetch the value mapped by a particular key mentioned in the parameter. It returns NULL when the map contains no such mapping for the key. |
| `hashCode()` | This method is used in Map Interface to generate a hashCode for the given map containing keys and values. |
| `isEmpty()` | This method is used to check if a map is having any entry for key and value pairs. If no mapping exists, then this returns true. |
| `keySet()` | This method is used in Map Interface to return a Set view of the keys contained in this map. The set is backed by the map, so changes to the map are reflected in the set, and vice-versa. |
| `put(Object, Object)` | This method is used in Java Map Interface to associate the specified value with the specified key in this map. |
| `putAll(Map)` | This method is used in Map Interface in Java to copy all of the mappings from the specified map to this map. |
| `remove(Object)` | This method is used in Map Interface to remove the mapping for a key from this map if it is present in the map. |
| `size()` | This method is used to return the number of key/value pairs available in the map. |
| `values()` | This method is used in Java Map Interface to create a collection out of the values of the map. It basically returns a Collection view of the values in the HashMap. |
| `getOrDefault(Object key, V defaultValue)` | Returns the value to which the specified key is mapped, or defaultValue if this map contains no mapping for the key. |
| `merge(K key, V value, BiFunction<? super V,? super V,? extends V> remappingFunction)` | If the specified key is not already associated with a value or is associated with null, associate it with the given non-null value. |
| `putIfAbsent(K key, V value)` | If the specified key is not already associated with a value (or is mapped to null) associates it with the given value and returns null, else returns the current associate value. |
## HashMap

In `HashMap` class uses a **hash table** for storage, every value can be accessed by its hash code, using `hashCode` by itself or overriding it with our implementation. Each access is done in $O(1)$ but it does **not maintain any order** of the keys; elements are unordered and can appear in any sequence.
## TreeMap
`TreeMap` uses a **Red-Black tree** (a self-balancing binary search tree) for storage. It maintains a **natural (sorted) order** of the keys by default, or you can provide a custom comparator for sorting. Keys are sorted either by their natural ordering (if the keys implement `Comparable`) or by a provided comparator.
## Key differences

| Feature            | HashMap                          | TreeMap                             |
|--------------------|----------------------------------|-------------------------------------|
| **Data Structure** | Hash Table                       | Red-Black Tree                      |
| **Order of Keys**  | Unordered                        | Sorted                              |
| **Null Key**       | Allows one `null` key            | Does not allow `null` keys          |
| **Performance**    | Fast for large data (O(1))       | Slower, maintains order (O(log n))  |
| **Use Case**       | Quick lookup without ordering    | Sorted data and range-based queries |

