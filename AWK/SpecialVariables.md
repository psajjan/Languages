# Special Variables in AWK

# FILENAME
It represents the current filename.

| Variable | Description |
|---|---|
| FILENAME | It represents the current filename |
| NF       | Number of fields in the current record/line |
| NR       | Current record/line number |
| FNR      | Similar to NR but its value resets with new file. |
| FS       | Input field separator and its default value is space |
| OFS      | Output field separator and its default value is space |
| RS       | Input record separator and its default value is newline |
| ORS      | Output record separator and its default value is newline |
| $0       | Entire input record |
| $n       | It is the nth field in the current record |
| ARGC     | It implies the number of arguments provided at the command line. |
| ARGV     | It is an array that stores the command-line arguments. The array's valid index ranges from 0 to ARGC-1. |
| ENVIRON  | It is an associative array of environment variables. |
| CONVFMT  | It represents the conversion format for numbers. Its default value is %.6g. |
| RLENGTH  | It represents the length of the string matched by match function. AWK's match function searches for a given string in the input-string. |
| RSTART   | It represents the first position in the string matched by match function. |
| SUBSEP   | It represents the separator character for array subscripts and its default value is \034. |
