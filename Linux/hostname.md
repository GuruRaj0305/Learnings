# Hostname

+ `/etc/hostname` -> this is the file where the hostname contains.
+ `hostnamectl set-hostname <new host name>` -> CMD to change host name.
    or change in `/etc/hostname` file and reboot.

# Finding system information 

+ `cat /etc/redhat-release` -> gives os name and version.
+ `uname -a` -> gives detail info of os.
+ `dmidecode` -> gives in detail info of everything (manufacturer, processor, memory, etc ...). 