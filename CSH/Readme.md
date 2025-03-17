# CSH Tips and Tricks

## String Related

### Converting string to array and access its words
Lets say there is a string with spaces in between and we want to access its individual words
To do that first we need to convert it into an array by putting it into the `()` operator and then access individual words using `[]` operator.

```csh
set mystr = "hello world thank you"
set myarr = ($mystr)
set word1 = $myarr[1]
set word2 = $myarr[2]
set word3 = $myarr[3]
set word4 = $myarr[4]
```

## Sub-String Extraction and Manipulation

C shell (csh) has a built-in way to manipulate strings using the `:` operator for substring extraction and modification within variable syntax.

### Substring Extraction with the `:` Operator
The `:` operator allows you to extract a portion of a string by specifying the starting index and length.
If you omit the length part, it extracts from start to the end of the string.

```csh
# $var[start:length]
# start is the zero-based index of the first character you want to extract.
# length is the number of characters to extract.

set mystring = "HelloWorld"
echo $mystring[1:5]   # Output: Hello
echo $mystring[6:5]   # Output: World

# Extract Rest of the string
echo $mystring[6:]    # Output: World
```
