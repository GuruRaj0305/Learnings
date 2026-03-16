## Amazon VPC

### Understanding CIDR – IPv4

- **CIDR** = Classless Inter-Domain Routing – a method for allocating IP addresses
- Used in Security Group rules and AWS networking in general
- IP address range examples:
  - `WW.XX.YY.ZZ/32` → one IP address
  - `0.0.0.0/0` → all IP addresses
  - `192.168.0.0/26` → 192.168.0.0 – 192.168.0.63 (64 IP addresses)

**CIDR Components:**
- **Base IP** – Represents an IP contained in the range (e.g., `10.0.0.0`, `192.168.0.0`)
- **Subnet Mask** – Defines how many bits can change in the IP (e.g., `/0`, `/24`, `/32`)

| Subnet Mask | Equivalent | Addresses |
| --- | --- | --- |
| /32 | 255.255.255.255 | 1 IP |
| /31 | 255.255.255.254 | 2 IPs |
| /30 | 255.255.255.252 | 4 IPs |
| /29 | 255.255.255.248 | 8 IPs |
| /28 | 255.255.255.240 | 16 IPs |
| /27 | 255.255.255.224 | 32 IPs |
| /26 | 255.255.255.192 | 64 IPs |
| /25 | 255.255.255.128 | 128 IPs |
| /24 | 255.255.255.0 | 256 IPs |
| /16 | 255.255.0.0 | 65,536 IPs |
| /8 | 255.0.0.0 | 16,777,216 IPs |
| /0 | 0.0.0.0 | All IPs |

**Quick memo:**
- `/32` – no octet can change
- `/24` – last octet can change
- `/16` – last 2 octets can change
- `/8` – last 3 octets can change
- `/0` – all octets can change

**CIDR Little Exercise:**
- `192.168.0.0/24` = 192.168.0.0 – 192.168.0.255 (256 IPs)
- `192.168.0.0/16` = 192.168.0.0 – 192.168.255.255 (65,536 IPs)
- `134.56.78.123/32` = just 134.56.78.123
- `0.0.0.0/0` = all IPs
- When in doubt: https://www.ipaddressguide.com/cidr

### Public vs. Private IP (IPv4)

- The Internet Assigned Numbers Authority (IANA) established certain blocks of IPv4 addresses for private (LAN) and public (Internet) use
- **Private IP ranges:**
  - `10.0.0.0 – 10.255.255.255` (`10.0.0.0/8`) – big networks
  - `172.16.0.0 – 172.31.255.255` (`172.16.0.0/12`) – AWS default VPC is in this range
  - `192.168.0.0 – 192.168.255.255` (`192.168.0.0/16`) – e.g., home networks
- All other IP addresses on the Internet are **Public**

### Default VPC

- All new AWS accounts have a default VPC
- New EC2 instances are launched into the default VPC if no subnet is specified
- Default VPC has Internet connectivity and all EC2 instances inside it have public IPv4 addresses
- We also get a public and a private IPv4 DNS name

### VPC in AWS – IPv4

- **VPC = Virtual Private Cloud**
- You can have multiple VPCs in an AWS region (max. 5 per region – soft limit)
- Max. 5 CIDRs per VPC, for each CIDR:
  - Min. size is /28 (16 IP addresses)
  - Max. size is /16 (65,536 IP addresses)
- Only the Private IPv4 ranges are allowed:
  - `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`
- Your VPC CIDR should NOT overlap with your other networks (e.g., corporate)

### VPC – Subnet (IPv4)

- AWS reserves **5 IP addresses** (first 4 & last 1) in each subnet
- These IPs are not available for use and cannot be assigned to EC2 instances
- **Example:** if CIDR block `10.0.0.0/24`, then reserved addresses are:
  - `10.0.0.0` – Network Address
  - `10.0.0.1` – reserved by AWS for the VPC router
  - `10.0.0.2` – reserved by AWS for mapping to Amazon-provided DNS
  - `10.0.0.3` – reserved by AWS for future use
  - `10.0.0.255` – Network Broadcast Address (not supported in VPC, reserved)

> **Exam Tip:** If you need 29 IP addresses for EC2 instances, you can’t choose /27 (32 - 5 = 27 < 29). You need /26 (64 - 5 = 59 > 29).

### Internet Gateway (IGW)

- Allows resources (e.g., EC2 instances) in a VPC to connect to the Internet
- Scales horizontally and is highly available and redundant
- Must be created separately from a VPC
- One VPC can only be attached to one IGW and vice versa
- Internet Gateways on their own do not allow Internet access — **Route Tables must also be edited!**

### Bastion Hosts

- Used to SSH into private EC2 instances
- The bastion is in the public subnet, which connects to all other private subnets
- **Bastion Host security group** must allow inbound from the internet on port 22 from a restricted CIDR (e.g., public CIDR of your corporation)
- **EC2 Instance security group** must allow the Security Group of the Bastion Host, or the private IP of the Bastion host

### NAT Instance (Outdated, but still on the exam)

- NAT = Network Address Translation
- Allows EC2 instances in private subnets to connect to the Internet
- Must be launched in a **public subnet**
- Must disable EC2 setting: **Source/Destination Check**
- Must have Elastic IP attached to it
- Route Tables must be configured to route traffic from private subnets to the NAT Instance
- Pre-configured Amazon Linux AMI is available (reached end of standard support on December 31, 2020)
- Not highly available/resilient out of the box — requires ASG in multi-AZ + resilient user-data script

**Security Group rules required:**
- Inbound: Allow HTTP/HTTPS from Private Subnets; Allow SSH from your home network
- Outbound: Allow HTTP/HTTPS to the Internet

### NAT Gateway

- AWS-managed NAT, higher bandwidth, high availability, no administration
- Pay per hour for usage and bandwidth
- NATGW is created in a specific Availability Zone, uses an Elastic IP
- Can’t be used by EC2 instances in the same subnet (only from other subnets)
- Requires an IGW (Private Subnet → NATGW → IGW)
- 5 Gbps of bandwidth with automatic scaling up to 100 Gbps
- No Security Groups to manage/required

**NAT Gateway with High Availability:**
- NAT Gateway is resilient within a single Availability Zone
- Must create multiple NAT Gateways in multiple AZs for fault-tolerance
- There is no cross-AZ failover needed (if an AZ goes down it doesn’t need NAT)

### NAT Gateway vs. NAT Instance

| Feature | NAT Gateway | NAT Instance |
| --- | --- | --- |
| Availability | Highly available within AZ (create in another AZ) | Must use a script to manage failover between instances |
| Bandwidth | Up to 100 Gbps | Depends on EC2 instance type |
| Maintenance | Managed by AWS | Managed by you (software, OS patches, etc.) |
| Cost | Per hour & amount of data transferred | Per hour, EC2 instance type and size + network $ |
| Public IPv4 | Yes | Yes |
| Private IPv4 | Yes | Yes |
| Security Groups | Not required | Required |
| Use as Bastion Host | No | Yes |

### Security Groups & NACLs

**Security Group (Stateful):**
- Operates at the instance level
- Supports Allow rules only
- Return traffic is automatically allowed regardless of any rules
- All rules are evaluated before deciding whether to allow traffic

**NACL (Stateless):**
- Operates at the subnet level
- Supports Allow and Deny rules
- Return traffic must be explicitly allowed by rules (think of ephemeral ports)
- Rules are evaluated in order (lowest to highest number); first match wins
- Automatically applies to all EC2 instances in the subnet

### Network Access Control List (NACL)

- NACLs are like a firewall which controls traffic from and to subnets
- One NACL per subnet; new subnets are assigned the Default NACL
- **NACL Rules:**
  - Rules have a number (1–32766); lower number = higher precedence
  - First rule match drives the decision
  - Example: Rule #100 ALLOW `10.0.0.10/32`, Rule #200 DENY `10.0.0.10/32` – IP is **allowed** (100 has higher precedence)
  - The last rule is an asterisk (`*`) and denies any request with no rule match
  - AWS recommends adding rules by increment of 100
- Newly created NACLs will deny everything
- Great way to block a specific IP address at the subnet level

### Default NACL

- Accepts everything inbound/outbound with the subnets it’s associated with
- Do NOT modify the Default NACL — instead create custom NACLs

**Default NACL Inbound Rules:**

| Rule # | Type | Protocol | Port Range | Source | Allow/Deny |
| --- | --- | --- | --- | --- | --- |
| 100 | All IPv4 Traffic | All | All | 0.0.0.0/0 | ALLOW |
| * | All IPv4 Traffic | All | All | 0.0.0.0/0 | DENY |

**Default NACL Outbound Rules:**

| Rule # | Type | Protocol | Port Range | Destination | Allow/Deny |
| --- | --- | --- | --- | --- | --- |
| 100 | All IPv4 Traffic | All | All | 0.0.0.0/0 | ALLOW |
| * | All IPv4 Traffic | All | All | 0.0.0.0/0 | DENY |

### Ephemeral Ports

- For any two endpoints to establish a connection, they must use ports
- Clients connect to a defined port, and expect a response on an **ephemeral port**
- Different Operating Systems use different port ranges:
  - IANA & MS Windows 10: 49,152 – 65,535
  - Many Linux Kernels: 32,768 – 60,999
- NACLs must allow traffic on ephemeral port ranges for return traffic

### Security Group vs. NACLs (Summary)

| Feature | Security Group | NACL |
| --- | --- | --- |
| Level | Instance level | Subnet level |
| Rule types | Allow only | Allow and Deny |
| Statefulness | Stateful – return traffic automatically allowed | Stateless – return traffic must be explicitly allowed |
| Rule evaluation | All rules evaluated before decision | Rules evaluated in order (lowest to highest); first match wins |
| Scope | Applies to an EC2 instance when specified | Automatically applies to all EC2 instances in associated subnet |

More info: https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html

### VPC Peering

- Privately connect two VPCs using AWS’s network
- Make them behave as if they were in the same network
- **Must not have overlapping CIDRs**
- VPC Peering connection is **NOT transitive** (must be established for each VPC pair)
- You must update route tables in each VPC’s subnets to ensure EC2 instances can communicate
- You can create VPC Peering connection between VPCs in different AWS accounts/regions
- You can reference a security group in a peered VPC (same region, cross accounts)

### VPC Endpoints (AWS PrivateLink)

- Every AWS service is publicly exposed (public URL)
- VPC Endpoints (powered by AWS PrivateLink) allows you to connect to AWS services using a **private network** instead of the public Internet
- Redundant and scale horizontally
- They remove the need of IGW, NATGW, etc. to access AWS Services
- In case of issues: Check DNS Setting Resolution in your VPC; Check Route Tables

**Types of Endpoints:**

| Type | Description | Cost |
| --- | --- | --- |
| Interface Endpoint (PrivateLink) | Provisions an ENI (private IP address) as an entry point. Must attach a Security Group. Supports most AWS services. | $ per hour + $ per GB |
| Gateway Endpoint | Provisions a gateway and must be used as a target in a route table. Supports only S3 and DynamoDB. | Free |

> **Gateway is most likely preferred at the exam.** Interface Endpoint is preferred when access is required from on-premises (Site-to-Site VPN or Direct Connect), a different VPC, or a different region.

### Lambda in VPC Accessing DynamoDB

- DynamoDB is a public AWS service
- **Option 1:** Access from the public internet – Lambda in VPC needs a NAT Gateway in a public subnet and an Internet Gateway
- **Option 2 (Better & Free):** Use a VPC Gateway Endpoint for DynamoDB and change the Route Tables

### VPC Flow Logs

- Captures information about IP traffic going into your interfaces:
  - VPC Flow Logs
  - Subnet Flow Logs
  - ENI (Elastic Network Interface) Flow Logs
- Helps to monitor & troubleshoot connectivity issues
- Flow logs data can go to S3, CloudWatch Logs, and Kinesis Data Firehose
- Captures network information from AWS managed interfaces too: ELB, RDS, ElastiCache, Redshift, WorkSpaces, NATGW, Transit Gateway

**Flow Log Syntax:**
`version | account-id | interface-id | srcaddr | dstaddr | srcport | dstport | protocol | packets | bytes | start | end | action | log-status`
- `srcaddr` & `dstaddr` – identify problematic IPs
- `srcport` & `dstport` – identify problematic ports
- `action` – success or failure (for Security Group/NACL)

**Troubleshoot SG & NACL issues using the ACTION field:**
- **Incoming:** Inbound REJECT → NACL or SG
- **Incoming:** Inbound ACCEPT, Outbound REJECT → NACL
- **Outgoing:** Outbound REJECT → NACL or SG
- **Outgoing:** Outbound ACCEPT, Inbound REJECT → NACL

**Architectures:** 
- VPC Flow Logs → CloudWatch Logs → Metric Filter → Alert
- VPC Flow Logs → S3 → Athena → QuickSight

**IAM Permissions required:** `logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents`

### AWS Site-to-Site VPN

- **Virtual Private Gateway (VGW):** VPN concentrator on the AWS side. Created and attached to the VPC from which you want to create the S2S VPN connection. Can customize ASN.
- **Customer Gateway (CGW):** Software application or physical device on the customer side of the VPN connection

**Site-to-Site VPN Connections:**
- Customer Gateway Device (On-premises): use the public Internet-routable IP address; if behind a NAT device, use the NAT device’s public IP
- **Important:** Enable Route Propagation for the Virtual Private Gateway in the route table associated with your subnets
- If you need to ping EC2 instances from on-premises, add the ICMP protocol on the inbound of your security groups

### AWS VPN CloudHub

- Provide secure communication between multiple sites (multiple VPN connections)
- Low-cost hub-and-spoke model for primary or secondary network connectivity between different locations (VPN only)
- Goes over the public Internet
- To set it up: connect multiple VPN connections on the same VGW, set up dynamic routing, configure route tables

### Direct Connect (DX)

- Provides a **dedicated private connection** from a remote network to your VPC
- Dedicated connection must be set up between your data center and AWS Direct Connect locations
- You need to set up a Virtual Private Gateway on your VPC
- Access public resources (S3) and private (EC2) on the same connection
- **Use Cases:**
  - Increase bandwidth throughput – working with large data sets – lower cost
  - More consistent network experience for applications using real-time data feeds
  - Hybrid Environments (on-premises + cloud)
- Supports both IPv4 and IPv6

**Direct Connect Gateway:**
- If you want to set up a Direct Connect to one or more VPCs in **many different regions** (same account), you must use a Direct Connect Gateway

**Direct Connect – Connection Types:**

| Type | Capacity | Notes |
| --- | --- | --- |
| Dedicated Connections | 1 Gbps, 10 Gbps, 100 Gbps | Physical Ethernet port dedicated to a customer. Requested to AWS first, then completed by AWS Direct Connect Partners |
| Hosted Connections | 50 Mbps to 10 Gbps | Via AWS Direct Connect Partners. Capacity can be added/removed on demand |

- Lead times are often longer than 1 month

**Direct Connect – Encryption:**
- Data in transit is **not encrypted** but is private
- AWS Direct Connect + VPN provides an **IPsec-encrypted private connection** (for extra security, but slightly more complex)

**Direct Connect – Resiliency:**
- **High Resiliency:** One connection at multiple Direct Connect locations
- **Maximum Resiliency:** Separate connections terminating on separate devices in more than one location

**Site-to-Site VPN as a Backup:**
- If Direct Connect fails, set up a backup Direct Connect connection (expensive), or a Site-to-Site VPN connection

### Transit Gateway

- For transitive peering between thousands of VPCs and on-premises, hub-and-spoke (star) connection
- Regional resource, can work cross-region
- Share cross-account using Resource Access Manager (RAM)
- You can peer Transit Gateways across regions
- Route Tables: limit which VPCs can talk with other VPCs
- Works with Direct Connect Gateway and VPN connections
- Supports IP Multicast (not supported by any other AWS service)

**Transit Gateway: Site-to-Site VPN ECMP:**
- **ECMP** = Equal-cost multi-path routing
- Allows forward packets over multiple best paths
- **Use case:** Create multiple Site-to-Site VPN connections to increase bandwidth to AWS
- Example throughput with ECMP:
  - 1 VPN connection = 1.25 Gbps (using 2 tunnels)
  - 2 VPN connections = 5.0 Gbps (ECMP)
  - 3 VPN connections = 7.5 Gbps (ECMP)

**Transit Gateway – Share Direct Connect between multiple accounts:**
- Use AWS Resource Access Manager to share Transit Gateway with other accounts

### VPC – Traffic Mirroring

- Allows you to capture and inspect network traffic in your VPC
- Route the traffic to security appliances that you manage
- **Capture the traffic:**
  - From (Source): ENIs
  - To (Targets): an ENI or a Network Load Balancer
- Capture all packets or specific packets of interest (optionally truncate)
- Source and Target can be in the same VPC or different VPCs (VPC Peering)
- **Use cases:** content inspection, threat monitoring, troubleshooting

### What is IPv6?

- IPv4 was designed to provide ~4.3 billion addresses (being exhausted)
- IPv6 is the successor of IPv4, providing 3.4 × 10^38 unique IP addresses
- Every IPv6 address in AWS is **public and Internet-routable** (no private range)
- Format: `x.x.x.x.x.x.x.x` (x is hexadecimal, range 0000–ffff)
- Examples:
  - `2001:db8:3333:4444:5555:6666:7777:8888`
  - `::` – all 8 segments are zero
  - `::1234:5678` – first 6 segments are zero

### IPv6 in VPC

- IPv4 cannot be disabled for your VPC and subnets
- You can enable IPv6 (public IP addresses) to operate in **dual-stack mode**
- Your EC2 instances will get at least a private internal IPv4 and a public IPv6
- They can communicate using either IPv4 or IPv6 to the internet through an Internet Gateway

**IPv4 Troubleshooting:**
- If you cannot launch an EC2 instance in your subnet, it’s NOT because of IPv6 (the space is very large)
- It’s because there are **no available IPv4 addresses** in your subnet
- **Solution:** Create a new IPv4 CIDR in your subnet

### Egress-only Internet Gateway

- Used for **IPv6 only** (similar to a NAT Gateway but for IPv6)
- Allows instances in your VPC outbound connections over IPv6 while preventing the internet from initiating an IPv6 connection to your instances
- You must update the Route Tables

### VPC Section Summary

**Core Components:**
- **CIDR** – IP Range
- **VPC** – Virtual Private Cloud; define a list of IPv4 & IPv6 CIDR
- **Subnets** – tied to an AZ; define a CIDR
- **Internet Gateway** – at the VPC level; provides IPv4 & IPv6 Internet Access
- **Route Tables** – must be edited to add routes from subnets to IGW, VPC Peering, VPC Endpoints
- **Bastion Host** – public EC2 instance to SSH into with SSH connectivity to private EC2 instances

**NAT:**
- **NAT Instances** – old; give Internet access to private EC2 instances; must be in public subnet; disable Source/Destination check
- **NAT Gateway** – managed by AWS; scalable Internet access for private EC2 instances (IPv4)

**Security:**
- **NACL** – stateless; subnet rules for inbound and outbound; don’t forget Ephemeral Ports
- **Security Groups** – stateful; operate at the EC2 instance level

**Connectivity:**
- **VPC Peering** – connect two VPCs with non-overlapping CIDR; non-transitive
- **VPC Endpoints** – provide private access to AWS Services (S3, DynamoDB, etc.) within a VPC
- **VPC Flow Logs** – can be set up at VPC/Subnet/ENI level for ACCEPT and REJECT traffic; analyze using Athena or CloudWatch Logs Insights
- **Site-to-Site VPN** – setup Customer Gateway on DC, Virtual Private Gateway on VPC, and S2S VPN over public Internet
- **AWS VPN CloudHub** – hub-and-spoke VPN model to connect your sites
- **Direct Connect** – setup Virtual Private Gateway on VPC and establish direct private connection to an AWS Direct Connect Location
- **Direct Connect Gateway** – Direct Connect to many VPCs in different AWS regions
- **AWS PrivateLink / VPC Endpoint Services** – connect services privately; needs NLB & ENI; no VPC Peering/NAT/Route Tables
- **Transit Gateway** – transitive peering for VPC, VPN & DX
- **Traffic Mirroring** – copy network traffic from ENIs for further analysis
- **Egress-only Internet Gateway** – like NAT Gateway but for IPv6 targets

### Networking Costs in AWS per GB (Simplified)

- Use **Private IP** instead of Public IP for cost savings and better network performance
- Use **same AZ** for maximum savings (at the cost of high availability)

| Traffic Type | Cost |
| --- | --- |
| Traffic in same AZ using private IP | Free |
| Traffic in same AZ using public/Elastic IP | $0.01 |
| Inter-AZ (same region) | $0.01 per GB |
| Inter-region | $0.02 per GB |

**Minimizing egress traffic cost:**
- **Egress traffic:** outbound traffic (AWS to outside) – costs are high
- **Ingress traffic:** inbound traffic (outside to AWS) – typically free
- Try to keep as much internet traffic within AWS to minimize costs
- Direct Connect locations co-located in the same AWS Region result in lower egress network costs

### S3 Data Transfer Pricing (USA)

| Transfer Type | Cost |
| --- | --- |
| S3 ingress | Free |
| S3 to Internet | $0.09 per GB |
| S3 Transfer Acceleration (additional) | +$0.04 to $0.08 per GB |
| S3 to CloudFront | $0.00 per GB |
| CloudFront to Internet | $0.085 per GB (slightly cheaper than S3, plus caching) |
| S3 Cross-Region Replication | $0.02 per GB |

### NAT Gateway vs. Gateway VPC Endpoint (Pricing)

| Feature | NAT Gateway | Gateway VPC Endpoint |
| --- | --- | --- |
| Cost per hour | $0.045 | Free |
| Data processed cost | $0.045 per GB | Free |
| Data transfer to S3 (same region) | $0.00 | $0.01 (in/out) |
| Data transfer to S3 (cross-region) | $0.09 | $0.01 (in/out) |

### Network Protection on AWS

To protect your VPC, AWS provides:
- Network Access Control Lists (NACLs)
- Amazon VPC Security Groups
- AWS WAF (protect against malicious requests)
- AWS Shield & AWS Shield Advanced
- AWS Firewall Manager (manage across accounts)
- **AWS Network Firewall** (for sophisticated VPC-wide protection)

### AWS Network Firewall

- Protect your entire Amazon VPC from Layer 3 to Layer 7
- Inspect any direction:
  - VPC to VPC traffic
  - Outbound to internet
  - Inbound from internet
  - To/from Direct Connect & Site-to-Site VPN
- Internally uses the AWS Gateway Load Balancer
- Rules can be centrally managed cross-account by AWS Firewall Manager

**Network Firewall – Fine Grained Controls:**
- Supports 1000s of rules
- IP & port filtering (e.g., 10,000s of IPs)
- Protocol filtering (e.g., block SMB protocol for outbound communications)
- Stateful domain list rule groups (e.g., only allow outbound to `*.mycorp.com`)
- General pattern matching using regex
- Traffic filtering: Allow, drop, or alert for matching traffic
- Active flow inspection with intrusion-prevention capabilities
- Send logs of rule matches to Amazon S3, CloudWatch Logs, Kinesis Data Firehose
