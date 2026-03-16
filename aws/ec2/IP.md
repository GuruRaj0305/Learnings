# Private vs Public IP (IPv4)

- Networking has two sorts of IPs: IPv4 and IPv6
  - IPv4: `1.160.10.240`
  - IPv6: `3ffe:1900:4545:3:200:f8ff:fe21:67cf`
- IPv4 is still the most common format used online
- IPv6 is newer and solves problems for the Internet of Things (IoT)
- IPv4 allows for 3.7 billion different addresses in the public space
- IPv4 format: `[0-255].[0-255].[0-255].[0-255]`

## Fundamental Differences

- **Public IP**:
  - The machine can be identified on the internet (WWW)
  - Must be unique across the whole web (no two machines can have the same public IP)
  - Can be geo-located easily
- **Private IP**:
  - The machine can only be identified on a private network
  - The IP must be unique across the private network
  - But two different private networks (two companies) can have the same IPs
  - Machines connect to the WWW using a NAT gateway (a proxy)
  - Only a specified range of IPs can be used as private IP

## Elastic IPs

- When you stop and then start an EC2 instance, it can change its public IP
- If you need a fixed public IP for your instance, you need an Elastic IP
- An Elastic IP is a public IPv4 you own as long as you don't delete it
- You can attach it to one instance at a time
- With an Elastic IP address, you can mask the failure of an instance or software by rapidly remapping the address to another instance in your account
- You can only have 5 Elastic IPs in your account (you can ask AWS to increase that)
- Overall, try to avoid using Elastic IP:
  - They often reflect poor architectural decisions
  - Instead, use a random public IP and register a DNS name to it
  - Or use a Load Balancer and don't use a public IP