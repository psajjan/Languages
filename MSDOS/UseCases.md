
## Copy files from sub folders to current folder

We are in a folder containing many sub-folders. Each sub-folder can have sub-folders recursively.  
There are files in every sub-folder. The goal is to move all the files from every sub-folder to current folder.

The way to do this is to first use a `FOR` loop to iterate over all the sub-folders recursively.
We use the `/D` flag to go over only folders and `/R` to recursively iterate over the sub-folders.

For each sub-folder, use second `FOR` loop to iterate over only files in that sub-folder.
Finally, use the `MOVE` command to move the file to the current directory.

If there are spaces in the file path then it must be wrapped using quotes.  
The current directory can be specified using `.` dot character.

```bat
@ECHO OFF
FOR /D /R %%i IN (*) DO (
  
  FOR %%j IN ("%%i\*") DO (
    MOVE "%%j" "."
  )
)
```
