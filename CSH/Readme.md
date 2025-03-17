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

C shell (csh) has a built-in way to manipulate strings using the `:` operator for **substring extraction** and **modification** within variable syntax.

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

### Modifiers
C shell (csh) provides a range of modifiers to manipulate strings in variable substitutions. These modifiers allow you to modify the value of a variable without altering its original content.

#### Modifiers for Substitution
These modifiers work on variables to transform their values dynamically.

`:r` — Remove the suffix (everything after the last dot):
```csh
set file = "document.txt"
echo $file:r    # Output: document
```

`:e` — Extract the suffix (everything after the last dot):
```csh
echo $file:e    # Output: txt
```

`:h` — Extract the directory (everything before the last /):
```csh
set path = "/home/user/file.txt"
echo $path:h    # Output: /home/user
```

`:t` — Extract the filename (everything after the last /):
```csh
echo $path:t    # Output: file.txt
```

#### Quoting Modifiers
These modifiers are helpful for ensuring that the values are safely quoted for specific operations.

`:q` — Quote the variable's value to escape characters for safe parsing:
```csh
set word = "Hello World"
echo $word:q    # Output: "Hello World"
```

`:x` — Similar to :q, but escapes the string for shell execution:
```csh
echo $word:x    # Output: Hello\ World
```

#### Default Value Modifiers
These modifiers allow you to define default values or fallback options for variables.

`:?` — If the variable is undefined, print an error message and exit the script.
```csh
echo $undefined_var:?  # Outputs an error message and exits.
```

`:d` — Use the default value if the variable is not set.
```csh
set myvar = ${undefined_var:d "DefaultValue"}
echo $myvar    # Output: DefaultValue
```
