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

```kotlin
for (i in 1..10) {
  println(i)
}
```
