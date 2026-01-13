# AWS Regions

* It is all around the world.
* Names can be us-east-1, eu-west-3...
* **A region is the cluster of data centers.**
* **Most aws services are region scoped**


## How to choose region?

* **Compliance** with data governance and legal requirement: Data never leaves a region with out your explicit permission.
* **Proximity** to customers: Reduce latency.
* **Available services** within a region: new services and features aren't available in every region.
* **Price** : varies to regions and it is transperent in service pricing page.


## AWS Availability Zones (AZ)

Each region has many availability zones ( Usually 3, max 6, min 3 )

Each availability zone is one or more discrete data centers with reduntant power, networking, and connectivity

They are seperate from each others, so that they'are isolated from disasters

They are connected with high bandwidth, ultra low latency network


## AWS Global services

### Identity & Access
- **IAM** – Users, roles, and policies
- **AWS Organizations** – Multi-account management
- **IAM Identity Center (AWS SSO)** – Centralized access management

### Networking & Delivery
- **Route 53** – Global DNS service
- **CloudFront** – Global content delivery network (CDN)

### Security
- **AWS WAF** – Global when associated with CloudFront
- **AWS Shield** – DDoS protection
- **AWS Firewall Manager** – Centralized firewall rule management

### Management / Account
- **Billing & Cost Management** – Account-wide billing and usage
- **Support Plans** – AWS support services

## Availability Zone (AZ)–Scoped

### Compute

- **EC2 Instance**  
  - Always launched in a specific Availability Zone



### Storage

- **EBS Volume**  
  - Tied to a single Availability Zone  
  - Cannot be attached to instances in other AZs



### Networking

- **Subnet**  
  - A subnet belongs to exactly **one Availability Zone**

- **NAT Gateway**  
  - Created in a single Availability Zone  
  - If the AZ fails, the NAT Gateway fails  
  - High availability requires one NAT Gateway per AZ



### Load Balancing (Important Nuance)

- **Load balancer nodes are AZ-specific**, even though:
  - **ALB** and **NLB** are **regional services**
  - Multiple Availability Zones must be enabled for high availability

## Regional service

Remaining all are regional service