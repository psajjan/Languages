# AWK

## String Functions

### <pre>gsub(regexp, replaceWith [, target]) - global substitution</pre>

Finds all the occurrances of the `regexp` in the `target` variable and replaces them with the given `replaceWith` string.  
It returns the number of substitutions made.

If the `target` variable is omitted, then the entire input record ($0) is used.
