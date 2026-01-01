# Process management

+ When any process is running then to shift that job to backgroung.  
    + **ctrl + z**  -> will stop the process
    + `jobs` -> to see all the process.
    + `bg` -> to send the most recent process to bg or  
    `bg %<number which showed in the jobs cmd>` will push that job to run in bg.

+ `fg` to bring job to foreground.
+ `nohup process &` -> to run process even after terminal exists. Or  
    `nohup process > /dev/null 2>&1 &` -> this will not show any error or standard output in terminal every thing moved to /dev/null.
+ `pkill <name>` -> kill process by name.
+ `nice -n <value between -20 to 19> command [arguments]` ->  to determine process priority. (+ve increase is less priority, -ve decreace is high priority).
+ `ps` -> List process.
+ `top` -> to monitor process.
