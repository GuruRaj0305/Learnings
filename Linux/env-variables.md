# Environment variables

+ `printevn` OR `env` ->  To view all environment variables  
+ `echo $SHELL` -> To view ONE environment variable
+ `export TEST=1` -> To set the environment variables, `echo $TEST` -> then to check.
+ To set environment variable permanently :
    open  `vi .bashrc`  
    ```shell
    TEST="123"
    export TEST
    ```
    -> add this at last
+ To set global environment variable permanently: same like above but add in `/etc/profile` or `/etc/bashrc` file.