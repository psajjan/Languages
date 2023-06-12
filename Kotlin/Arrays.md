# Arrays

## Array class

Arrays in Kotlin are represented using the `Array` class. The elements of the array can be accessed using the `[]` operator.
First element is located at the `0th` index. The `[]` operator statements are turned into calls to `get()` and `set()` member functions.
The number of elements in the array is given by the `size` property.

To create an array, we can use the function `arrayOf()` and pass it the item values.

```kotlin
val arr = arrayOf(0, 4, 9, 11)
arr.forEach { println(it) }
```
