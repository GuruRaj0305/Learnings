# Private vs Public IP (IPv4)

+ Networking has two sorts of IPs - IPv4 and IPv6
  + IPv4: 1.260.10.240
  + IPv6: 3ffe:1900:4545:3:200:f8ff:fe21:67f
+ IPv4 is still most common format used online
+ IPv6 is newer and solves problems for the internet of things (IoT).
+ IPv4 allows for 3.7 billion different addersses in the public space
+ IPv4: [0-255].[0-255].[0-255].[0-255].


+ Fundamental Differences: 
  + **Public IP** : 
    + Public IP means the mechine can be identified on the internet (www)
    + Must be unique across the whole web (not two machines can have the same public IP).
    + can be geo-located easily.
  + **Private IP**:
    + Pricate IP means the machine can only be identified on a private networ only
    + The IP must be unique across the private network 
    + But two different private networks (two companies ) can have the same IPs.
    + Machines connect to www using an internet gateways (a proxy).
    + Only a specified range of IPs can be used as private IP


## Elastic IPs

+ When you stop and then start an EC2 instance, it can change its public IP
+ If you need to have a fixed public IP for your instance, you need an Elastic IP
+ An Elastic IP is a public IPv4 you own as long as you don't delete it
+ Yor can attach it to one instance at a time
+ With an Elastic IP address, you can mask the failure of an instance or software by rapidly remapping the address to another instance in your account.
+ You can only have 5 Elastic IP in your account (you can ask AWS to increase that).
+ Overall, try to avoid using Elastic IP:
  + They often refelect poor architectural decisions
  + Instead, use a random public IP and register a DNS name to it.
  + Or, use a Load BAlancer and dont use a public IP