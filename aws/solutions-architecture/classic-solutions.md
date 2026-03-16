## Classic Solutions Architecture


### Section Introduction

- These solutions architectures are the best part of this course
- Let’s understand how all the technologies we’ve seen work together
- Sample case studies:
  - WhatIsTheTime.Com
  - MyClothes.Com
  - MyWordPress.Com
  - Instantiating applications quickly
  - Elastic Beanstalk


### Stateless Web App: WhatIsTheTime.com

- WhatIsTheTime.com allows people to know what time it is
- We don’t need a database
- We want to start small and can accept downtime
- We want to fully scale vertically and horizontally, no downtime

**Evolution of the architecture:**

1. **Starting simple:** Single Public EC2 with an Elastic IP
2. **Scaling vertically:** Upgrade EC2 instance type (downtime while upgrading)
3. **Scaling horizontally:** Multiple EC2 instances, Route 53 A Record (TTL 1 hour) pointing to public IPs – problem: removing instances causes DNS cache to point to gone IPs
4. **Add ELB + Health Checks:** Route 53 Alias Record → ELB; EC2 instances in private subnets; restricted Security Group rules; no more stale IPs
5. **Add Auto Scaling Group:** ASG manages EC2 instances behind ELB; scale in/out automatically
6. **Make it multi-AZ:** ELB spans multiple AZs; ASG deploys across at least 3 AZs
7. **Reserve capacity:** Reserve minimum 2 instances (Reserved Instances) for cost savings


### What We Learned from WhatIsTheTime.com

- Public vs Private IP and EC2 instances
- Elastic IP vs Route 53 vs Load Balancers
- Route 53 TTL, A Records and Alias Records
- Maintaining EC2 instances manually vs Auto Scaling Groups
- Multi-AZ to survive disasters
- ELB Health Checks
- Security Group Rules
- Reservation of capacity for cost savings when possible


### Stateful Web App: MyClothes.com

- MyClothes.com allows people to buy clothes online
- There’s a shopping cart
- Our website is having hundreds of users at the same time
- We need to scale, maintain horizontal scalability and keep our web application as stateless as possible
- Users should not lose their shopping cart
- Users should have their details (address, etc) in a database

**Evolution of the architecture:**

1. **Basic multi-AZ setup:** ELB → Auto Scaling Group across 3 AZs – problem: shopping cart lives in EC2 memory; different requests hit different instances
2. **Introduce Stickiness (Session Affinity):** ELB Stickiness routes user to same EC2 instance – problem: if instance fails, cart is lost
3. **Introduce User Cookies:** Shopping cart content stored in HTTP cookies (stateless HTTP requests) – security risk (cookies can be altered); must be validated; cookies < 4 KB
4. **Introduce Server Session:** Send `session_id` in cookie; store/retrieve session data from ElastiCache (or DynamoDB); stateless EC2 instances
5. **Store User Data in Database:** Store address, name, etc. in Amazon RDS
6. **Scaling Reads:** Add RDS Read Replicas; or Lazy Loading via ElastiCache (cache RDS results)
7. **Multi-AZ:** ElastiCache Multi-AZ, RDS Multi-AZ for disaster recovery
8. **Security Groups:** Restrict traffic to ElastiCache from EC2 SG only; restrict traffic to RDS from EC2 SG only; open HTTP/HTTPS to 0.0.0.0/0 only on ELB SG


### What We Learned from MyClothes.com

- 3-tier architectures for web applications
- ELB sticky sessions
- Web clients for storing cookies (stateless web app)
- ElastiCache for storing sessions (alternative: DynamoDB) and caching RDS data
- RDS for storing user data, Read Replicas for scaling reads, Multi-AZ for disaster recovery
- Tight Security with security groups referencing each other


### Stateful Web App: MyWordPress.com

- We are trying to create a fully scalable WordPress website
- We want that website to access and correctly display picture uploads
- Our user data, and the blog content should be stored in a MySQL database

**Evolution of the architecture:**

1. **RDS layer:** Multi-AZ RDS for WordPress database
2. **Aurora Multi-AZ & Read Replicas:** Replace RDS with Aurora MySQL for better scalability and HA
3. **Storing images with EBS:** Images stored in EBS volume – problem: EBS is tied to one AZ; if traffic hits a different AZ, images not accessible
4. **Storing images with EFS:** Replace EBS with EFS; EFS is distributed across AZs; all EC2 instances share the same file system


### What We Learned from MyWordPress.com

- Aurora Database for easy Multi-AZ and Read-Replicas
- Storing data in EBS (single instance application – tied to one AZ)
- vs. Storing data in EFS (distributed application – shared across AZs)


### Instantiating Applications Quickly

When launching a full stack (EC2, EBS, RDS), it can take time to install applications, insert initial data, configure everything, and launch the application. We can take advantage of the cloud to speed that up:

- **EC2 Instances:**
  - **Golden AMI:** Install your applications, OS dependencies etc. beforehand and launch your EC2 instance from the Golden AMI
  - **Bootstrap using User Data:** For dynamic configuration, use User Data scripts
  - **Hybrid:** Mix Golden AMI and User Data (Elastic Beanstalk)
- **RDS Databases:**
  - Restore from a snapshot: the database will have schemas and data ready!
- **EBS Volumes:**
  - Restore from a snapshot: the disk will already be formatted and have data!


### Typical Architecture: Web App 3-Tier

```
Route 53
  ↓
ELB (Multi-AZ)
  ↓
Auto Scaling Group (EC2 instances – Private Subnet)
  ↓                    ↓
ElastiCache          Amazon RDS
(Session / Cache)    (Read / Write)
    PUBLIC SUBNET  |  PRIVATE SUBNET  |  DATA SUBNET
```


### Developer Problems on AWS

- Managing infrastructure
- Deploying Code
- Configuring all the databases, load balancers, etc
- Scaling concerns
- Most web apps have the same architecture (ALB + ASG)
- All the developers want is for their code to run!


### Elastic Beanstalk – Overview

- Elastic Beanstalk is a developer-centric view of deploying an application on AWS
- It uses all the components we’ve seen before: EC2, ASG, ELB, RDS, …
- Managed service: automatically handles capacity provisioning, load balancing, scaling, application health monitoring, instance configuration, …
- Just the application code is the responsibility of the developer
- We still have full control over the configuration
- Beanstalk is free but you pay for the underlying instances


### Elastic Beanstalk – Components

- **Application:** Collection of Elastic Beanstalk components (environments, versions, configurations, …)
- **Application Version:** An iteration of your application code
- **Environment:**
  - Collection of AWS resources running an application version (only one application version at a time)
  - Tiers: Web Server Environment Tier & Worker Environment Tier
  - You can create multiple environments (dev, test, prod, …)

Workflow: `Create Application → Upload Version → Launch Environment → Manage Environment` (update version → deploy new version)


### Elastic Beanstalk – Supported Platforms

- Go
- Java SE
- Java with Tomcat
- .NET Core on Linux
- .NET on Windows Server
- Node.js
- PHP
- Python
- Ruby
- Packer Builder
- Single Container Docker
- Multi-container Docker
- Preconfigured Docker


### Web Server Tier vs. Worker Tier

| Tier | Description |
| --- | --- |
| **Web Server Tier** | Handles HTTP/HTTPS requests; EC2 instances behind an ELB; Auto Scaling Group |
| **Worker Tier** | Processes background jobs from an SQS Queue; EC2 instances (workers) pull from SQS; scales based on number of SQS messages |

- Can push messages to SQS queue from a Web Server Tier to trigger Worker Tier processing


### Elastic Beanstalk – Deployment Modes

| Mode | Use Case | Description |
| --- | --- | --- |
| **Single Instance** | Dev/test | Single EC2 with Elastic IP; single RDS instance |
| **High Availability with Load Balancer** | Production | ALB + Auto Scaling Group across multiple AZs; RDS Multi-AZ |
