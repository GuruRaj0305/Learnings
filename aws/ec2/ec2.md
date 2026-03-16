# EC2 (Elastic Compute Cloud)

- EC2 = Elastic Compute Cloud = Infrastructure as a Service
- EC2 is one of the most popular AWS offerings
- It mainly consists of the capability of:
  - Renting virtual machines (EC2)
  - Storing data on virtual drives (EBS)
  - Distributing load across machines (ELB)
  - Scaling the services using an auto-scaling group (ASG)

## EC2 Configuration Options

- OS: Linux, Windows, Mac OS
- How much compute power & cores (CPU)
- How much RAM
- How much storage space:
  - Network-attached (EBS & EFS)
  - Hardware (EC2 Instance Store)
- Network card: speed of the card, Public IP address
- Firewall rules: security group
- Bootstrap script (configure at first launch): EC2 User Data

## EC2 User Data

- It is possible to bootstrap instances using an EC2 User Data script
- **Bootstrapping** means launching commands when a machine starts
- That script is **only run once** at the instance **first start**
- EC2 User Data is used to automate boot tasks such as:
  - Installing updates
  - Installing software
  - Downloading common files from the internet
  - Anything you need
- The EC2 User Data Script runs with the root user


