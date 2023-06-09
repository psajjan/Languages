# All About Loops

## For loop

### Basic syntax

The `for` loop can be used to iterate over anything that provides an iterator.  
The general syntax of `for` is the following:

```kotlin
// Single line
for (item in collection) print(item)

// Body can be provided as a block
for (item in collection) {
  println(item)
}
```

### Iterate over a range

Iterate over numbers from 1 upto and including 10. By default the step size is 1.
```kotlin
// By default step size is 1
for (i in 1..10) {
  println(i)
}

// Specify a custom step size
for (i in 1..10 step 2) {
  print("$i ")
}
```

Iterate over a range in decreasing order. When going down over the step is always a positive number.
```kotlin
for (i in 100 downto 1 step 10) {
  print("$i  ")
}
```

## Iterate over an array

Iterate over the elements of an array using implicit iterator.
```kotlin
val myArr = arrayOf(100, 200, 300)
for (num in myArr) println(num)
```

We can iterate over an array using the `indices` property.
```kotlin
val myArr = arrayOf("john", "lee", "bob")
for (i in myArr.indices) prinln("Element at $i is ${myArr[i]}")
```

It is also possible to iterate over index value pairs using `withIndex()` api.
```kotlin
val myArr = arrayOf("a", "b", "c")
for ((i, v) in myArr.withIndex()) println("Element at $i is $v")
```
