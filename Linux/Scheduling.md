# Cron tab CMD

Used to schedule tasks.
Each user can have their own crontab file.
The **cron daemon (crond)**  checks these files and executes tasks at the scheduled time.

## Crontab Commands

+   `crontab -e` → Edit the current user’s crontab.
    inside this we will write cmd to run  
    0 0 0 0 0 command_to_run  
    │ │ │ │ │  
    │ │ │ │ └── Day of week (0-6, Sun=0 or 7)  
    │ │ │ └──── Month (1-12)  
    │ │ └────── Day of month (1-31)  
    │ └──────── Hour (0-23)  
    └────────── Minute (0-59)  

+ `crontab -l` → List current user’s cron jobs.
+ `crontab -r` → Remove all cron jobs for current user.
+ `crontab -u username -e` → Edit another user’s cron jobs (requires root).

+ `systemctl status crond` -> We can activate or deactivate cron jobs (for ubuntu its cron)

---

# Cron Job Directories

The another way of scheduling the jobs.  
Linux provides predefined directories where scripts can be placed to run automatically without editing crontab.

## Cron Directories

+ /etc/cron.hourly/ → files inside this will Runs every hour.
+ /etc/cron.daily/ → files inside this will Runs once per day.
+ /etc/cron.weekly/ → files inside this will Runs once per week.
+ /etc/cron.monthly/ → files inside this will Runs once per month.

> **Any script placed inside these directories will run at the corresponding interval, without needing to edit crontab.**

> we can check and alter when in daily weekly monthly will run the job in `/etc/anacrontab` and for hourly jobs it is `/etc/cron.d 0hourly`

#### Note : 
+ Scripts must be executable (chmod +x script.sh).
---

# at CMD

The **at** command is used to schedule one-time tasks in Linux.  
Unlike cron (repetitive), at runs a command once at a specific time.  
When the command is run it will be in intractive mode and you can get out by **ctrl + D**
Requires the **atd** service to be running.


> **Syntax** : `at [OPTION] TIME` 


### Usage 
+ `at <HH>:<MM>PM` -> to schedule a job.  
    Then will type actual cmd to run at that time.
+ `atq` -> List the pending jobs
+ `atrm <number of job>` -> to remove any job 
+ `systemctl statur atd` -> to check the status of this service