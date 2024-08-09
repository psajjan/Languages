# AWK

## Associative Arrays

### Accessing/Assigning in Arrays
The general way of using an array to access one of its element looks like the following.
The `index` can be anything, an integer or a string.
<pre>
  array[index]
</pre>

If you try to access an element that doesn't exists, then values returned is `""` (empty string).  
This includes elements to which you have not assigned any value, and elements that have been deleted.  
Such a reference automatically creates that array element, with the null string as its value.

Assigning a value to an array element is done using:
<pre>array[index] = value</pre>

You can remove an individual element of an array using the delete statement:
<pre>delete array[index]</pre>

You can delete all the elements of an array with a single statement, by leaving off the subscript in the delete statement.
<pre>delete array</pre>

### Testing if an index exists
You can find out if an element exists in an array at a certain index with the expression.
<pre>index in array</pre>
This expression tests whether or not the particular index exists, without the side effect of creating that element if it is not present.  
The expression has the value one (true) if `array[index]` exists, and zero (false) if it does not exist.

We can use the above expression in `if` statements.
<pre>
  if ("key1" in myHash) {
    print "Subscript key1 exists";
  }
</pre>
**NOTE** There is no way to find out if a particular value exists in the array or not, except to scan all the elements.

### Scanning all the elements
Awk has a special kind of `for` statement for scanning an array:
<pre>
  for (key in array)
    body using array[key]
</pre>

## String Functions

### <pre>gsub(regexp, replaceWith [, target]) - global substitution</pre>

Finds all the occurrances of the `regexp` in the `target` variable and replaces them with the given `replaceWith` string.  
It returns the number of substitutions made.

If the `target` variable is omitted, then the entire input record ($0) is used.
