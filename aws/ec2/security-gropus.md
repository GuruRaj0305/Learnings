# Security Groups

+ Security Groups are the fundamental of network security in AWS
+ They control how traffic is allowed into or out of EC2 instances.
+ Security groups only contain allow rules
+ Security groups can reference by IP or by security group (can referance each other)


## Security Groups (Deep Dive) 
+ Security groups are acting as a firewall on EC2 instances
+ They regulates:
  + Access to ports
  + Authorised IP ranges - IPv4 and IPv6 
  + Control of inbound network (from other to the instance) 
  + Control of outbound network (from the instance to other)



+ Locked down to a region / VPC combination 
+ If your application is not accessible (time out), then it's a security group issue
+ By default all inbound traffic is blocked and all outbound traffic is authorised.


## Classic Ports to know

+ 22 => SSH (Secure shell) - login into a linux instance
+ 21 => FTP (File Transfer Protocol) - upload files into a files into a file share
+ 22 => SFTP (Secure File Transfer Protocol ) - upload files using SSH
+ 80 => HTTP - access unsecured websites
+ 443 => HTTPs - access secured websites
+ 3389 => RDP (Remote Desktop Protocol) - log into a windows instance



```
Never ever add your IAM User Creds inside the instance Instead Create IAM Role and add that role to instance.
```


