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
