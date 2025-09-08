# System Monitoring

+ `top` -> Used to monitor the process.
+ `df` -> (disk free) to monitor disk memory and partitions in your system.  
    `df -h` -> in human readable.  
    `df -T` → show filesystem type (ext4, xfs, tmpfs…).
+ `dmesg` -> to see all the error and warnings which system generated.
+ `iostat` -> Useful for finding I/O bottlenecks and monitoring system performance.  
    `iostat 1` -> to refresh every single min.
+ `ip` -> **managing and displaying network interfaces, routing, and IP addresses**.    
    It comes from the `iproute2` package and replaces older tools like `ifconfig` and `route`.
+ `ss` -> (socket statistics) command is used to display information about sockets (TCP, UDP, UNIX).
    It is faster and more detailed than netstat.
+ `free` -> Displays the **amount of free and used memory** in the system. Helps monitor memory usage in real time. 
    `free -h`→ human-readable (MB, GB).
+ `cat /proc/cpuinfo` -> Shows **detailed information about the CPU(s)**. Useful for checking CPU model, cores, cache size, and flags.
+ `cat /proc/meminfo` -> detailed memory usage breakdown.

# Log Monitoring
All logs is logged here untill the application specifies any external directory.

**The directory => `/var/log/`**

### some of the important log files are : 
+ boot -> to see all the boot related logs.
+ cron -> to see all the cron jobs exicuted details log.( when and which job exicuted )
+ maillog -> All mail related logs here 
+ securelog -> all user login logout related log is generated here.( when ever user logins, logouts and incorrect password etc. )
+ > messages -> this is important log where it stores what ever machine related log is stored here ( all the hardware info logs, software info logs, all the application info logs, processes ingo logs )
