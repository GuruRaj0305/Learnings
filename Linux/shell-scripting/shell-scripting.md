# Shell scripting
A shell scripting is an executable file that contains multiple shell commands that are executed sequentially.

## structure
+ shell (#!/bin/bash)
+ comments (# your comment like this) 
+ commands (ls, echo, grep etc.)
+ statements (if, while, for etc)


Shell scripting file should always have executable permission.



### Usage 

+ `filename.sh` -> is the extension (if no extension also works)
+ `chmod +x filename.sh` -> it should be always executable 


#### Variables

``` sh
readonly PI=3.14 # this variable only for read we cant over write again
name="Guru"
echo "My name is $name"

unset name # unset the varaible 
```


#### User Input

```sh
echo "Enter your name:"
read name # read from user
echo "Hello $name"

read -sp "Password: " pass # read without showing passwd

```

#### Arithmetic
