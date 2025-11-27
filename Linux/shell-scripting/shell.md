# Shell

Interface between user and kernel/OS


+ `echo $0` -> to show which is current shell
+ `cat /etc/shells` -> to show all available shells
+ `grep <username> /etc/passwd` -> to see what is the default shell of the user.


## To change default shell
change manually in /etc/passwd or `sudo chsh -s /bin/bash <username>` or for current user `chsh -s /bin/bash`


## Types of shell
+ Gnome
+ KDE
+ sh
+ bash (born again shell)
+ zsh
+ csh and tcsh (for using in c and c++)