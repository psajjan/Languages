# Basic Commands
## Clear the screen
Command `CLS` is used to clear the screen.

## Prevent echo of command
Every command that is run is printed on screen, which can make analyzing the real output difficult.
There is a way to turn off echoing of the command that is run. Issue `@ECHO OFF` to turn off the printing of command.
```bat
@ECHO OFF
ECHO "The only output"
```

# For Loop
## Basic syntax

Following is the basic syntax of the `for` command.

```batch
FOR %%parameter IN (set) DO (
  command
  command
)
```

Following is one example to iterate over a set of words.

```batch
FOR %%i IN (cat dog hen) do (
  ECHO The %%i is an animal. 
)
```

### Iterate over only files
Use the following command to iterate over only files in the current directory.
```batch
FOR %%i IN (*) DO ECHO %%i
```

Use the following command to iterate over only files in the current and all sub directories.
The flag `/R` has to be used to iterate recursively.
```batch
FOR /R %%i IN (*) DO ECHO %%i
```

Iterate over only files from all sub directories except the files in current directory.  
For this, we can first iterate over all the sub directories using the `/D` flag and recurse using `/R` flag.  
Then iterate over only files in each of these sub directories.
```bat
FOR /D /R %%i IN (*) DO (
  FOR %%j IN ("%%i/*") DO ECHO %%j
)
```

### Iterate over only directories
Use the following command to iterate over only folders in the current directory.
The flag `/D` has to be used to iterate over only folders.
```batch
FOR /D %%i IN (*) DO ECHO %%i
```

Use the following command to iterate over only folders in the current and all sub directories.
The flag `/R` has to be used to iterate recursively.
```batch
FOR /D /R %%i IN (*) DO ECHO %%i
```
