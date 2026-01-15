# EC2 (Elastic Compute Cloud)

+ EC2 = Elastic Compute Cloud = Infrastructure as a service.
+ EC2 is one of the most popular of AWS offering.
+ It mainly consists in the capability of:
  + Renting virtual machines (EC2)
  + Storing data on virtual drives (EBS)
  + Distributing load across machines (ELB)
  + Scaling the services using an auto-scaling group (ASG)


## EC2 configuration options
+ OS : Linux, Windows, Mac Os
+ How much compute power & cores (CPU)
+ How much RAM 
+ How much storage space : 
  + Network-attached (EBS & EFS)
  + hardware (EC2 instance store)
+ Network card : speed of the card, Public IP address
+ Firewall rules : security group
+ Bootstrap script (configure at first launch) : EC2 User Data


## EC2 User Data 
+ It is possible to bootstrap our instances using an EC2 User data script.
+ **bootstrapping** means launching commands when a machine starts
+ That sript is **only run once** at the instance **first start**
+ EC2 user data is used to automate boot tasks such as:
  + Installing updates
  + Installing software
  + Downloading common files from internet 
  + Anything ...
+ The EC2 User Data Script runs with the root user.
  


