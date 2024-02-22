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
