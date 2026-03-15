# AWS SAA-C03 – Study Notes Index

> Course by Stéphane Maarek.  
> Each section has its own file in the folder listed below.

## Sections

| # | Topic | Folder / File |
|---|-------|---------------|
| 1 | Getting Started with AWS + IAM | [getting-started/getting-started.md](getting-started/getting-started.md) |
| 2 | Amazon EC2 – Basics | [ec2/01-ec2-basics.md](ec2/01-ec2-basics.md) |
| 3 | Amazon EC2 – Associate | [ec2/02-ec2-associate.md](ec2/02-ec2-associate.md) |
| 4 | Amazon EC2 – Instance Storage | [ec2/03-ec2-instance-storage.md](ec2/03-ec2-instance-storage.md) |
| 5 | High Availability & Scalability (ELB + ASG) | [ha-scalability/ha-scalability.md](ha-scalability/ha-scalability.md) |
| 6 | RDS, Aurora & ElastiCache | [rds/rds-aurora-elasticache.md](rds/rds-aurora-elasticache.md) |
| 7 | Amazon Route 53 | [route53/route53.md](route53/route53.md) |
| 8 | Classic Solutions Architecture | [solutions-architecture/classic-solutions.md](solutions-architecture/classic-solutions.md) |
| 9 | Amazon S3 – Basics | [s3/01-s3-basics.md](s3/01-s3-basics.md) |
| 10 | Amazon S3 – Advanced | [s3/02-s3-advanced.md](s3/02-s3-advanced.md) |
| 11 | Amazon S3 – Security | [s3/03-s3-security.md](s3/03-s3-security.md) |
| 12 | CloudFront & Global Accelerator | [cloudfront/cloudfront.md](cloudfront/cloudfront.md) |
| 13 | AWS Storage Extras | [storage/storage-extras.md](storage/storage-extras.md) |
| 14 | AWS Integration & Messaging (SQS, SNS, Kinesis) | [messaging/messaging.md](messaging/messaging.md) |
| 15 | Containers on AWS | [containers/containers.md](containers/containers.md) |
| 16 | Serverless Overview | [serverless/01-serverless-overview.md](serverless/01-serverless-overview.md) |
| 17 | Serverless Architectures | [serverless/02-serverless-architectures.md](serverless/02-serverless-architectures.md) |
| 18 | Databases in AWS | [databases/databases.md](databases/databases.md) |
| 19 | Data & Analytics | [analytics/data-analytics.md](analytics/data-analytics.md) |
| 20 | Machine Learning | [ml/machine-learning.md](ml/machine-learning.md) |
| 21 | AWS Monitoring, Audit & Performance | [monitoring/monitoring.md](monitoring/monitoring.md) |
| 22 | Advanced Identity in AWS | [identity/advanced-identity.md](identity/advanced-identity.md) |
| 23 | AWS Security & Encryption | [security/security-encryption.md](security/security-encryption.md) |
| 24 | Amazon VPC | [vpc/vpc.md](vpc/vpc.md) |
| 25 | Disaster Recovery & Migrations | [disaster-recovery/disaster-recovery.md](disaster-recovery/disaster-recovery.md) |
| 26 | More Solutions Architecture | [more-solutions/more-solutions.md](more-solutions/more-solutions.md) |
| 27 | AWS Cost Management | [cost-management/cost-explorer.md](cost-management/cost-explorer.md) |
| 28 | Exam Review & Tips | [exam-prep/exam-review.md](exam-prep/exam-review.md) |

---

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