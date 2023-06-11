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

### Files in current directory
Use the following command to iterate over only files in the current directory.
```batch
FOR %%i IN (*) DO ECHO %%i
```

### Files in current and sub directories
Use the following command to iterate over only files in the current and all sub directories.
```batch
FOR /R %%i IN (*) DO ECHO %%i
```
