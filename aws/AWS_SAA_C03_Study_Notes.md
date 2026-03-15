# AWS Certified Solutions Architect Associate – SAA-C03
**Complete Study Notes** | Course by Stéphane Maarek

---

## Table of Contents

- [Getting Started with AWS](#getting-started-with-aws)
- [AWS Identity & Access Management (IAM)](#aws-identity--access-management-iam)
- [Amazon EC2 – Basics](#amazon-ec2--basics)
- [Amazon EC2 – Associate](#amazon-ec2--associate)
- [Amazon EC2 – Instance Storage](#amazon-ec2--instance-storage)
- [High Availability & Scalability](#high-availability--scalability)
- [RDS, Aurora & ElastiCache](#rds-aurora--elasticache)
- [Amazon Route 53](#amazon-route-53)
- [Classic Solutions Architecture](#classic-solutions-architecture)
- [Amazon S3](#amazon-s3)
- [Amazon S3 – Advanced](#amazon-s3--advanced)
- [Amazon S3 – Security](#amazon-s3--security)
- [CloudFront & Global Accelerator](#cloudfront--global-accelerator)
- [AWS Storage Extras](#aws-storage-extras)
- [AWS Integration & Messaging](#aws-integration--messaging)
- [Containers on AWS](#containers-on-aws)
- [Serverless Overview](#serverless-overview)
- [Serverless Architectures](#serverless-architectures)
- [Databases in AWS](#databases-in-aws)
- [Data & Analytics](#data--analytics)
- [Machine Learning](#machine-learning)
- [AWS Monitoring, Audit & Performance](#aws-monitoring-audit--performance)
- [Advanced Identity in AWS](#advanced-identity-in-aws)
- [AWS Security & Encryption](#aws-security--encryption)
- [Amazon VPC](#amazon-vpc)
- [Disaster Recovery & Migrations](#disaster-recovery--migrations)
- [More Solutions Architecture](#more-solutions-architecture)
- [Other Services](#other-services)
- [White Papers & Architectures](#white-papers--architectures)
- [Exam Preparation](#exam-preparation)

---


---

## Getting Started with AWS


### AWS Cloud History

2002: 2004: 2007:

Internally Launched publicly Launched in

launched with SQS Europe

2003: 2006:

Amazon infrastructure is Re-launched

one of their core strength. publicly with

Idea to market SQS, S3 & EC2


### AWS Cloud Number Facts

- In 2023, AWS had $90 billion

in annual revenue

- AWS accounts for 31% of the

market in Q1 2024 (Microsoft

is 2nd with 25%)

- Pioneer and Leader of the

AWS Cloud Market for the

13th consecutive year

- Over 1,000,000 active users

Gartner Magic Quadrant


### AWS Cloud Use Cases

- AWS enables you to build sophisticated, scalable applications
- Applicable to a diverse set of industries
- Use cases include
- Enterprise IT, Backup & Storage, Big Data analytics
- Website hosting, Mobile & Social Apps
- Gaming

### AWS Global Infrastructure

- AWS Regions
- AWS Availability Zones
- AWS Data Centers
- AWS Edge Locations /

Points of Presence

- https://infrastructure.aws/

### AWS Regions

- AWS has Regions all around the world
- Names can be us-east-1, eu-west-3…
- A region is a cluster of data centers
- Most AWS services are region-scoped

https://aws.amazon.com/about-aws/global-infrastructure/


### How to choose an AWS Region?

If you need to launch a new application,

- Compliance with data governance and legal

where should you do it?

requirements: data never leaves a region without

your explicit permission

- Proximity to customers: reduced latency
- Available services within a Region: new services

and new features aren’t available in every Region

- Pricing: pricing varies region to region and is

transparent in the service pricing page


### AWS Availability Zones

- Each region has many availability zones

AWS Region

(usually 3, min is 3, max is 6). Example:

Sydney: ap-southeast-2

- ap-southeast-2a
- ap-southeast-2b

ap-southeast-2a

- ap-southeast-2c
- Each availability zone (AZ) is one or more

discrete data centers with redundant power,

networking, and connectivity

- They’re separate from each other, so that

ap-southeast-2b ap-southeast-2c

they’re isolated from disasters

- They’re connected with high bandwidth,

ultra-low latency networking


### AWS Points of Presence (Edge Locations)

- Amazon has 400+ Points of Presence (400+ Edge Locations & 10+

Regional Caches) in 90+ cities across 40+ countries

- Content is delivered to end users with lower latency

https://aws.amazon.com/cloudfront/features/


### Tour of the AWS Console

- AWS has Global Services:
- Identity and Access Management (IAM)
- Route 53 (DNS service)
- CloudFront (Content Delivery Network)
- WAF (Web Application Firewall)
- Most AWS services are Region-scoped:
- Amazon EC2 (Infrastructure as a Service)
- Elastic Beanstalk (Platform as a Service)
- Lambda (Function as a Service)
- Rekognition (Software as a Service)
- Region Table:

https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services


### AWS Identity and Access

Management (AWS IAM)


### IAM: Users & Groups

- IAM = Identity and Access Management, Global service
- Root account created by default, shouldn’t be used or shared
- Users are people within your organization, and can be grouped
- Groups only contain users, not other groups
- Users don’t have to belong to a group, and user can belong to multiple groups

Group: Developers Group: Operations

Group

Audit Team

Alice Bob Charles David Edward Fred


### IAM: Permissions

"Version": "2012-10-17",

- Users or Groups can be "Statement": [

assigned JSON documents

"Effect": "Allow",

"Action": "ec2:Describe*",

called policies

"Resource": "*"

- These policies define the {

"Effect": "Allow",

permissions of the users "Action": "elasticloadbalancing:Describe*",

"Resource": "*"

- In AWS you apply the least },

privilege principle: don’t give "Effect": "Allow",

"Action": [

more permissions than a user

"cloudwatch:ListMetrics",

"cloudwatch:GetMetricStatistics",

needs

"cloudwatch:Describe*"

"Resource": "*"


### IAM Policies inheritance

Audit Team

Developers Operations

inline

Alice Bob Charles David Edward Fred


### IAM Policies Structure

- Consists of
- Version: policy language version, always include “2012-10-
- Id: an identifier for the policy (optional)
- Statement: one or more individual statements (required)
- Statements consists of
- Sid: an identifier for the statement (optional)
- Effect: whether the statement allows or denies access

(Allow, Deny)

- Principal: account/user/role to which this policy applied to
- Action: list of actions this policy allows or denies
- Resource: list of resources to which the actions applied to
- Condition: conditions for when this policy is in effect

(optional)


### IAM – Password Policy

- Strong passwords = higher security for your account
- In AWS, you can setup a password policy:
- Set a minimum password length
- Require specific character types:
- including uppercase letters
- lowercase letters
- numbers
- non-alphanumeric characters
- Allow all IAM users to change their own passwords
- Require users to change their password after some time (password expiration)
- Prevent password re-use

### Multi Factor Authentication - MFA

- Users have access to your account and can possibly change

configurations or delete resources in your AWS account

- You want to protect your Root Accounts and IAM users
- MFA = password you know + security device you own

+ =>

Password Successful login

Alice

- Main benefit of MFA:

if a password is stolen or hacked, the account is not compromised


### MFA devices options in AWS

Virtual MFA device Universal 2nd Factor (U2F) Security Key

YubiKey by Yubico (3rd party)

Google Authenticator Authy

(phone only) (phone only)

Support for multiple root and IAM users

Support for multiple tokens on a single device.

using a single security key


### MFA devices options in AWS

Hardware Key Fob MFA Device for

Hardware Key Fob MFA Device

AWS GovCloud (US)

Provided by Gemalto (3rd party) Provided by SurePassID (3rd party)


### How can users access AWS ?

- To access AWS, you have three options:
- AWS Management Console (protected by password + MFA)
- AWS Command Line Interface (CLI): protected by access keys
- AWS Software Developer Kit (SDK) - for code: protected by access keys
- Access Keys are generated through the AWS Console
- Users manage their own access keys
- Access Keys are secret, just like a password. Don’t share them
- Access Key ID ~= username
- Secret Access Key ~= password

### Example (Fake) Access Keys

- Access key ID: AKIASK4E37PV4983d6C
- Secret Access Key: AZPN3zojWozWCndIjhB0Unh8239a1bzbzO5fqqkZq
- Remember: don’t share your access keys

### What’s the AWS CLI?

- A tool that enables you to interact with AWS services using commands in

your command-line shell

- Direct access to the public APIs of AWS services
- You can develop scripts to manage your resources
- It’s open-source https://github.com/aws/aws-cli
- Alternative to using AWS Management Console

### What’s the AWS SDK?

- AWS Software Development Kit (AWS SDK)
- Language-specific APIs (set of libraries)
- Enables you to access and manage AWS services

programmatically

AWS SDK

- Embedded within your application
- Supports
- SDKs (JavaScript, Python, PHP, .NET, Ruby, Java, Go, Node.js,

C++)

- Mobile SDKs (Android, iOS, …)

Your Application

- IoT Device SDKs (Embedded C, Arduino, …)
- Example: AWS CLI is built on AWS SDK for Python

### IAM Roles for Ser vices

IAM Role

- Some AWS service will need to

perform actions on your behalf

- To do so, we will assign EC2 Instance

permissions to AWS services (virtual server)

with IAM Roles

- Common roles:
- EC2 Instance Roles Access AWS
- Lambda Function Roles
- Roles for CloudFormation

### IAM Security Tools

- IAM Credentials Report (account-level)
- a report that lists all your account's users and the status of their various

credentials

- IAM Access Advisor (user-level)
- Access advisor shows the service permissions granted to a user and when those

services were last accessed.

- You can use this information to revise your policies.

### IAM Guidelines & Best Practices

- Don’t use the root account except for AWS account setup
- One physical user = One AWS user
- Assign users to groups and assign permissions to groups
- Create a strong password policy
- Use and enforce the use of Multi Factor Authentication (MFA)
- Create and use Roles for giving permissions to AWS services
- Use Access Keys for Programmatic Access (CLI / SDK)
- Audit permissions of your account using IAM Credentials Report & IAM

Access Advisor

- Never share IAM users & Access Keys

### IAM Section – Summary

- Users: mapped to a physical user, has a password for AWS Console
- Groups: contains users only
- Policies: JSON document that outlines permissions for users or groups
- Roles: for EC2 instances or AWS services
- Security: MFA + Password Policy
- AWS CLI: manage your AWS services using the command-line
- AWS SDK: manage your AWS services using a programming language
- Access Keys: access AWS using the CLI or SDK
- Audit: IAM Credential Reports & IAM Access Advisor

---

## Amazon EC2 – Basics


### Amazon EC2

- EC2 is one of the most popular of AWS’ offering
- EC2 = Elastic Compute Cloud = Infrastructure as a Service
- It mainly consists in the capability of :
- Renting virtual machines (EC2)
- Storing data on virtual drives (EBS)
- Distributing load across machines (ELB)
- Scaling the services using an auto-scaling group (ASG)
- Knowing EC2 is fundamental to understand how the Cloud works

### EC2 sizing & configuration options

- Operating System (OS): Linux, Windows or Mac OS
- How much compute power & cores (CPU)
- How much random-access memory (RAM)
- How much storage space:
- Network-attached (EBS & EFS)
- hardware (EC2 Instance Store)
- Network card: speed of the card, Public IP address
- Firewall rules: security group
- Bootstrap script (configure at first launch): EC2 User Data

### EC2 User Data

- It is possible to bootstrap our instances using an EC2 User data script.
- bootstrapping means launching commands when a machine starts
- That script is only run once at the instance first start
- EC2 user data is used to automate boot tasks such as:
- Installing updates
- Installing software
- Downloading common files from the internet
- Anything you can think of
- The EC2 User Data Script runs with the root user

### Hands-On:

Launching an EC2 Instance running Linux

- We’ll be launching our first virtual server using the AWS Console
- We’ll get a first high-level approach to the various parameters
- We’ll see that our web server is launched using EC2 user data
- We’ll learn how to start / stop / terminate our instance.

### EC2 Instance Types - Over view

- You can use different types of EC2 instances that are optimised for

different use cases (https://aws.amazon.com/ec2/instance-types/)

- AWS has the following naming convention:

m5.2xlarge

- m: instance class
- 5: generation (AWS improves them over time)
- 2xlarge: size within the instance class

### EC2 Instance Types – General Purpose

- Great for a diversity of workloads such as web servers or code repositories
- Balance between:
- Compute
- Memory
- Networking
- In the course, we will be using the t2.micro which is a General Purpose EC2

instance

* this list will evolve over time, please check the AWS website for the latest information


### EC2 Instance Types – Compute Optimized

- Great for compute-intensive tasks that require high performance

processors:

- Batch processing workloads
- Media transcoding
- High performance web servers
- High performance computing (HPC)
- Scientific modeling & machine learning
- Dedicated gaming servers

* this list will evolve over time, please check the AWS website for the latest information


### EC2 Instance Types – Memor y Optimized

- Fast performance for workloads that process large data sets in memory
- Use cases:
- High performance, relational/non-relational databases
- Distributed web scale cache stores
- In-memory databases optimized for BI (business intelligence)
- Applications performing real-time processing of big unstructured data

* this list will evolve over time, please check the AWS website for the latest information


### EC2 Instance Types – Storage Optimized

- Great for storage-intensive tasks that require high, sequential read and write

access to large data sets on local storage

- Use cases:
- High frequency online transaction processing (OLTP) systems
- Relational & NoSQL databases
- Cache for in-memory databases (for example, Redis)
- Data warehousing applications
- Distributed file systems

* this list will evolve over time, please check the AWS website for the latest information


### EC2 Instance Types: example

Storage Network EBS Bandwidth

Instance vCPU Mem (GiB)

Performance (Mbps)

t2.micro 1 1 EBS-Only Low to Moderate

t2.xlarge 4 16 EBS-Only Moderate

c5d.4xlarge 16 32 1 x 400 NVMe SSD Up to 10 Gbps 4,750

r5.16xlarge 64 512 EBS Only 20 Gbps 13,600

m5.8xlarge 32 128 EBS Only 10 Gbps 6,800

Great website: https://instances.vantage.sh


| Storage Network EBS Bandwidth Instance vCPU Mem (GiB) Performance (Mbps) |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| t2.micro | 1 | 1 | EBS-Only | Low to Moderate |  |
| t2.xlarge | 4 | 16 | EBS-Only | Moderate |  |
| c5d.4xlarge | 16 | 32 | 1 x 400 NVMe SSD | Up to 10 Gbps | 4,750 |
| r5.16xlarge | 64 | 512 | EBS Only | 20 Gbps | 13,600 |
| m5.8xlarge | 32 | 128 | EBS Only | 10 Gbps | 6,800 |


### Introduction to Security Groups

- Security Groups are the fundamental of network security in AWS
- They control how traffic is allowed into or out of our EC2 Instances.

Inbound traffic

Outbound traffic

- Security groups only contain rules
- Security groups rules can reference by IP or by security group

ytiruceS

puorG

WWW EC2 Instance


| yt p | EC2 Instance |
| --- | --- |
| iruceS uorG |  |


### Security Groups

Deeper Dive

- Security groups are acting as a “firewall” on EC2 instances
- They regulate:
- Access to Ports
- Authorised IP ranges – IPv4 and IPv6
- Control of inbound network (from other to the instance)
- Control of outbound network (from the instance to other)

### Security Groups

Diagram

Your Computer - IP XX.XX.XX.XX

Security Group 1 Port 22 (authorised port 22)

Inbound

Other computer

Filter IP / Port with Rules Port 22

(not authorised port 22)

EC2 Instance

IP XX.XX.XX.XX

Security Group 1

Outbound

Any Port

Any IP – Any Port

Filter IP / Port with Rules


|  |  |  |
| --- | --- | --- |
|  | Security Group 1 | Port 22 |
|  | Inbound Filter IP / Port with Rules |  |


|  |  | Security Group 1 |
| --- | --- | --- |
|  |  | Outbound |
|  |  | Filter IP / Port with Rules |


### Security Groups

Good to know

- Can be attached to multiple instances
- Locked down to a region / VPC combination
- Does live “outside” the EC2 – if traffic is blocked the EC2 instance won’t see it
- It’s good to maintain one separate security group for SSH access
- If your application is not accessible (time out), then it’s a security group issue
- If your application gives a “connection refused“ error, then it’s an application

error or it’s not launched

- All inbound traffic is blocked by default
- All outbound traffic is authorised by default

### Referencing other security groups

Diagram

Security

EC2 Instance

Port 123 Group 2

IP XX.XX.XX.XX

(attached)

Security Group 1

Security

EC2 Instance Inbound EC2 Instance

Port 123 Group 1

IP XX.XX.XX.XX

Authorising Security Group 1 IP XX.XX.XX.XX

(attached)

Authorising Security Group 2

Security

EC2 Instance

Port 123 Group 3

IP XX.XX.XX.XX

(attached)


| Security Group 2 (attached) |  |
| --- | --- |


|  |  |  |
| --- | --- | --- |
|  |  | Port 123 |
|  | Security Group 1 Inbound |  |
|  |  | Port 123 |
|  | Authorising Security Group 1 Authorising Security Group 2 |  |


| Security Group 1 (attached) |  |
| --- | --- |


| Security Group 3 (attached) |  |
| --- | --- |


### Classic Por ts to know

- 22 = SSH (Secure Shell) - log into a Linux instance
- 21 = FTP (File Transfer Protocol) – upload files into a file share
- 22 = SFTP (Secure File Transfer Protocol) – upload files using SSH
- 80 = HTTP – access unsecured websites
- 443 = HTTPS – access secured websites
- 3389 = RDP (Remote Desktop Protocol) – log into a Windows instance

### SSH Summary Table

EC2 Instance

SSH Putty

Connect

Linux

Windows < 10

Windows >= 10


### Which Lectures to watch

- Mac / Linux:
- SSH on Mac/Linux lecture
- Windows:
- Putty Lecture
- If Windows 10: SSH on Windows 10 lecture
- All:
- EC2 Instance Connect lecture

### SSH troubleshooting

- Students have the most problems with SSH
- If things don’t work…

1. Re-watch the lecture. You may have missed something

2. Read the troubleshooting guide

3. Try EC2 Instance Connect

- If one method works (SSH, Putty or EC2 Instance Connect) you’re good
- If no method works, that’s okay, the course won’t use SSH much

### How to SSH into your EC2 Instance

Linux / Mac OS X

- We’ll learn how to SSH into your EC2 instance using Linux / Mac
- SSH is one of the most important function. It allows you to control a

remote machine, all using the command line.

EC2 Instance

Linux

Public IP

- We will see how we can configure OpenSSH ~/.ssh/config to facilitate

the SSH into our EC2 instances

Port


### How to SSH into your EC2 Instance

Windows

- We’ll learn how to SSH into your EC2 instance using Windows
- SSH is one of the most important function. It allows you to control a

remote machine, all using the command line.

EC2 Instance

Linux

Public IP

- We will configure all the required parameters necessary for doing SSH

on Windows using the free tool Putty.

Port


### EC2 Instance Connect

- Connect to your EC2 instance within your browser
- No need to use your key file that was downloaded
- The “magic” is that a temporary key is uploaded onto EC2 by AWS
- Works only out-of-the-box with Amazon Linux 2
- Need to make sure the port 22 is still opened!

### EC2 Instances Purchasing Options

- On-Demand Instances – short workload, predictable pricing, pay by second
- Reserved (1 & 3 years)
- Reserved Instances – long workloads
- Convertible Reserved Instances – long workloads with flexible instances
- Savings Plans (1 & 3 years) –commitment to an amount of usage, long workload
- Spot Instances – short workloads, cheap, can lose instances (less reliable)
- Dedicated Hosts – book an entire physical server, control instance placement
- Dedicated Instances – no other customers will share your hardware
- Capacity Reservations – reserve capacity in a specific AZ for any duration

### EC2 On Demand

- Pay for what you use:
- Linux or Windows - billing per second, after the first minute
- All other operating systems - billing per hour
- Has the highest cost but no upfront payment
- No long-term commitment
- Recommended for short-term and un-interrupted workloads, where

you can't predict how the application will behave


### EC2 Reser ved Instances

- Up to 72% discount compared to On-demand
- You reserve a specific instance attributes (Instance Type, Region, Tenancy, OS)
- Reservation Period – 1 year (+discount) or 3 years (+++discount)
- Payment Options – No Upfront (+), Partial Upfront (++), All Upfront (+++)
- Reserved Instance’s Scope – Regional or Zonal (reserve capacity in an AZ)
- Recommended for steady-state usage applications (think database)
- You can buy and sell in the Reserved Instance Marketplace
- Convertible Reserved Instance
- Can change the EC2 instance type, instance family, OS, scope and tenancy
- Up to 66% discount

Note: the % discounts are different from the video as AWS

change them over time – the exact numbers are not needed

for the exam. This is just for illustrative purposes J


### EC2 Savings Plans

- Get a discount based on long-term usage (up to 72% - same as RIs)
- Commit to a certain type of usage ($10/hour for 1 or 3 years)
- Usage beyond EC2 Savings Plans is billed at the On-Demand price
- Locked to a specific instance family & AWS region (e.g., M5 in us-east-1)
- Flexible across:
- Instance Size (e.g., m5.xlarge, m5.2xlarge)
- OS (e.g., Linux, Windows)
- Tenancy (Host, Dedicated, Default)

### EC2 Spot Instances

- Can get a discount of up to 90% compared to On-demand
- Instances that you can “lose” at any point of time if your max price is less than the

current spot price

- The MOST cost-efficient instances in AWS
- Useful for workloads that are resilient to failure
- Batch jobs
- Data analysis
- Image processing
- Any distributed workloads
- Workloads with a flexible start and end time
- Not suitable for critical jobs or databases

### EC2 Dedicated Hosts

- A physical server with EC2 instance capacity fully dedicated to your use
- Allows you address compliance requirements and use your existing server-

bound software licenses (per-socket, per-core, pe—VM software licenses)

- Purchasing Options:
- On-demand – pay per second for active Dedicated Host
- Reserved - 1 or 3 years (No Upfront, Partial Upfront, All Upfront)
- The most expensive option
- Useful for software that have complicated licensing model (BYOL – Bring Your

Own License)

- Or for companies that have strong regulatory or compliance needs

### EC2 Dedicated Instances

- Instances run on hardware that’s

dedicated to you

- May share hardware with other

instances in same account

- No control over instance placement

(can move hardware after Stop / Start)


### EC2 Capacity Reser vations

- Reserve On-Demand instances capacity in a specific AZ for any duration
- You always have access to EC2 capacity when you need it
- No time commitment (create/cancel anytime), no billing discounts
- Combine with Regional Reserved Instances and Savings Plans to benefit

from billing discounts

- You’re charged at On-Demand rate whether you run instances or not
- Suitable for short-term, uninterrupted workloads that needs to be in a

specific AZ


### Which purchasing option is right for me?

- On demand: coming and staying in resort

whenever we like, we pay the full price

- Reserved: like planning ahead and if we plan to

stay for a long time, we may get a good discount.

- Savings Plans: pay a certain amount per hour for

certain period and stay in any room type (e.g.,

King, Suite, Sea View, …)

- Spot instances: the hotel allows people to bid for

the empty rooms and the highest bidder keeps the

rooms. You can get kicked out at any time

- Dedicated Hosts: We book an entire building of

the resort

- Capacity Reservations: you book a room for a

period with full price even you don’t stay in it


### Price Comparison

Example – m4.large – us-east-1

Price Type Price (per hour)

On-Demand $0.10

Spot Instance (Spot Price) $0.038 - $0.039 (up to 61% off)

Reserved Instance (1 year) $0.062 (No Upfront) - $0.058 (All Upfront)

Reserved Instance (3 years) $0.043 (No Upfront) - $0.037 (All Upfront)

EC2 Savings Plan (1 year) $0.062 (No Upfront) - $0.058 (All Upfront)

Reserved Convertible Instance (1 year) $0.071 (No Upfront) - $0.066 (All Upfront)

Dedicated Host On-Demand Price

Dedicated Host Reservation Up to 70% off

Capacity Reservations On-Demand Price


| Price Type | Price (per hour) |
| --- | --- |
| On-Demand | $0.10 |
| Spot Instance (Spot Price) | $0.038 - $0.039 (up to 61% off) |
| Reserved Instance (1 year) | $0.062 (No Upfront) - $0.058 (All Upfront) |
| Reserved Instance (3 years) | $0.043 (No Upfront) - $0.037 (All Upfront) |
| EC2 Savings Plan (1 year) | $0.062 (No Upfront) - $0.058 (All Upfront) |
| Reserved Convertible Instance (1 year) | $0.071 (No Upfront) - $0.066 (All Upfront) |
| Dedicated Host | On-Demand Price |
| Dedicated Host Reservation | Up to 70% off |
| Capacity Reservations | On-Demand Price |


### EC2 Spot Instance Requests

- Can get a discount of up to 90% compared to On-demand
- Define max spot price and get the instance while current spot price < max
- The hourly spot price varies based on offer and capacity
- If the current spot price > your max price you can choose to stop or terminate your instance

with a 2 minutes grace period.

- Other strategy: Spot Block
- “block” spot instance during a specified time frame (1 to 6 hours) without interruptions
- In rare situations, the instance may be reclaimed
- Used for batch jobs, data analysis, or workloads that are resilient to failures.
- Not great for critical jobs or databases

### EC2 Spot Instances Pricing

User-defined max price

https://console.aws.amazon.com/ec2sp/v1/spot/home?region=us-east-1#


### How to terminate Spot Instances?

You can only cancel Spot Instance requests that are open, active, or disabled.

Cancelling a Spot Request does not terminate instances

You must first cancel a Spot Request, and then terminate the associated Spot Instances

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-requests.html


### Spot Fleets

- Spot Fleets = set of Spot Instances + (optional) On-Demand Instances
- The Spot Fleet will try to meet the target capacity with price constraints
- Define possible launch pools: instance type (m5.large), OS, Availability Zone
- Can have multiple launch pools, so that the fleet can choose
- Spot Fleet stops launching instances when reaching capacity or max cost
- Strategies to allocate Spot Instances:
- lowestPrice: from the pool with the lowest price (cost optimization, short workload)
- diversified: distributed across all pools (great for availability, long workloads)
- capacityOptimized: pool with the optimal capacity for the number of instances
- priceCapacityOptimized (recommended): pools with highest capacity available, then select

the pool with the lowest price (best choice for most workloads)

- Spot Fleets allow us to automatically request Spot Instances with the lowest price

---

## Amazon EC2 – Associate


### Private vs Public IP (IPv4)

- Networking has two sorts of IPs. IPv4 and IPv6:
- IPv4: 1.160.10.240
- IPv6: 3ffe:1900:4545:3:200:f8ff:fe21:67cf
- In this course, we will only be using IPv4.
- IPv4 is still the most common format used online.
- IPv6 is newer and solves problems for the Internet of Things (IoT).
- IPv4 allows for 3.7 billion different addresses in the public space
- IPv4: [0-255].[0-255].[0-255].[0-255].

### Private vs Public IP (IPv4)

Example

Web Server (public): Server (public):

79.216.59.75 211.139.37.43

Internet Gateway (public): Internet Gateway (public):

149.140.72.10 253.144.139.205

Company A Company B

Private Network Private Network

192.168.0.1/22 192.168.0.1/22


### Private vs Public IP (IPv4)

Fundamental Differences

- Public IP:
- Public IP means the machine can be identified on the internet (WWW)
- Must be unique across the whole web (not two machines can have the same public IP).
- Can be geo-located easily
- Private IP:
- Private IP means the machine can only be identified on a private network only
- The IP must be unique across the private network
- BUT two different private networks (two companies) can have the same IPs.
- Machines connect to WWW using a NAT + internet gateway (a proxy)
- Only a specified range of IPs can be used as private IP

### Elastic IPs

- When you stop and then start an EC2 instance, it can change its public
- If you need to have a fixed public IP for your instance, you need an

Elastic IP

- An Elastic IP is a public IPv4 IP you own as long as you don’t delete it
- You can attach it to one instance at a time

### Elastic IP

- With an Elastic IP address, you can mask the failure of an instance or software

by rapidly remapping the address to another instance in your account.

- You can only have 5 Elastic IP in your account (you can ask AWS to increase

that).

- Overall, try to avoid using Elastic IP:
- They often reflect poor architectural decisions
- Instead, use a random public IP and register a DNS name to it
- Or, as we’ll see later, use a Load Balancer and don’t use a public IP

### Private vs Public IP (IPv4)

In AWS EC2 – Hands On

- By default, your EC2 machine comes with:
- A private IP for the internal AWS Network
- A public IP, for the WWW.
- When we are doing SSH into our EC2 machines:
- We can’t use a private IP, because we are not in the same network
- We can only use the public IP.
- If your machine is stopped and then started,

the public IP can change


### Placement Groups

- Sometimes you want control over the EC2 Instance placement strategy
- That strategy can be defined using placement groups
- When you create a placement group, you specify one of the following

strategies for the group:

- Cluster—clusters instances into a low-latency group in a single Availability Zone
- Spread—spreads instances across underlying hardware (max 7 instances per

group per AZ)

- Partition—spreads instances across many different partitions (which rely on

different sets of racks) within an AZ. Scales to 100s of EC2 instances per group

(Hadoop, Cassandra, Kafka)


### Placement Groups

Cluster

EC2 EC2 EC2

Placement group

Cluster

Same AZ

Low latency

10 Gbps network

EC2 EC2 EC2

- Pros: Great network (10 Gbps bandwidth between instances with Enhanced

Networking enabled - recommended)

- Cons: If the AZ fails, all instances fails at the same time
- Use case:
- Big Data job that needs to complete fast
- Application that needs extremely low latency and high network throughput

| EC2 |  |
| --- | --- |
|  |  |
| EC2 |  |


| EC2 |  |
| --- | --- |
|  |  |
| EC2 |  |


### Placement Groups

Spread

- Pros:

Us-east-1a Us-east-1b Us-east-1c

- Can span across Availability

Zones (AZ)

- Reduced risk is simultaneous

failure

EC2 EC2 EC2

- EC2 Instances are on different

physical hardware

Hardware 1 Hardware 3 Hardware 5

- Cons:
- Limited to 7 instances per AZ

per placement group

- Use case:

EC2 EC2 EC2 • Application that needs to

maximize high availability

- Critical Applications where

Hardware 2 Hardware 4 Hardware 6

each instance must be isolated

from failure from each other


### Placements Groups

Par tition

- Up to 7 partitions per AZ

us-east-1a us-east-1b

- Can span across multiple AZs in the

same region

EC2 EC2 EC2

- Up to 100s of EC2 instances
- The instances in a partition do not

share racks with the instances in the

EC2 EC2 EC2

other partitions

- A partition failure can affect many

EC2 EC2 EC2 EC2 but won’t affect other partitions

- EC2 instances get access to the

partition information as metadata

EC2 EC2 EC2

- Use cases: HDFS, HBase, Cassandra,

Partition 1 Partition 2 Partition 3 Kafka


### Elastic Network Interfaces (ENI)

- Logical component in a VPC that represents a

virtual network card

Availability Zone

- The ENI can have the following attributes:

Eth0 – primary ENI

- Primary private IPv4, one or more secondary IPv4 192.168.0.31
- One Elastic IP (IPv4) per private IPv4

Eth1 – secondary ENI

- One Public IPv4 192.168.0.42
- One or more security groups

Can be moved

- A MAC address
- You can create ENI independently and attach

Eth0 – primary ENI

them on the fly (move them) on EC2 instances EC2

for failover

- Bound to a specific availability zone (AZ)

### EC2 Hibernate

- We know we can stop, terminate instances
- Stop – the data on disk (EBS) is kept intact in the next start
- Terminate – any EBS volumes (root) also set-up to be destroyed is lost
- On start, the following happens:
- First start: the OS boots & the EC2 User Data script is run
- Following starts: the OS boots up
- Then your application starts, caches get warmed up, and that can take time!

### EC2 Hibernate

EC2 Instance

Running

- Introducing EC2 Hibernate: Root EBS Volume

(Encrypted)

- The in-memory (RAM) state is preserved

Hibernate

- The instance boot is much faster!

(the OS is not stopped / restarted)

Stopping

- Under the hood: the RAM state is written

to a file in the root EBS volume

- The root EBS volume must be encrypted Shutdown

Hibernation

- Use cases:

Stopped

- Long-running processing
- Saving the RAM state Start
- Services that take time to initialize

Running


### EC2 Hibernate – Good to know

- Supported Instance Families – C3, C4, C5, I3, M3, M4, R3, R4, T2, T3, …
- Instance RAM Size – must be less than 150 GB.
- Instance Size – not supported for bare metal instances.
- AMI – Amazon Linux 2, Linux AMI, Ubuntu, RHEL, CentOS & Windows…
- Root Volume – must be EBS, encrypted, not instance store, and large
- Available for On-Demand, Reserved and Spot Instances
- An instance can NOT be hibernated more than 60 days

---

## Amazon EC2 – Instance Storage


### What’s an EBS Volume?

- An EBS (Elastic Block Store) Volume is a network drive you can attach

to your instances while they run

- It allows your instances to persist data, even after their termination
- They can only be mounted to one instance at a time (at the CCP level)
- They are bound to a specific availability zone
- Analogy: Think of them as a “network USB stick”

### EBS Volume

- It’s a network drive (i.e. not a physical drive)
- It uses the network to communicate the instance, which means there might be a bit of

latency

- It can be detached from an EC2 instance and attached to another one quickly
- It’s locked to an Availability Zone (AZ)
- An EBS Volume in us-east-1a cannot be attached to us-east-1b
- To move a volume across, you first need to snapshot it
- Have a provisioned capacity (size in GBs, and IOPS)
- You get billed for all the provisioned capacity
- You can increase the capacity of the drive over time

### EBS Volume - Example

US-EAST-1A US-EAST-1B

EBS EBS EBS EBS EBS

(10 GB) (100 GB) (50 GB) (50 GB) (10 GB)

unattached


### EBS – Delete on Termination attribute

- Controls the EBS behaviour when an EC2 instance terminates
- By default, the root EBS volume is deleted (attribute enabled)
- By default, any other attached EBS volume is not deleted (attribute disabled)
- This can be controlled by the AWS console / AWS CLI
- Use case: preserve root volume when instance is terminated

### EBS Snapshots

- Make a backup (snapshot) of your EBS volume at a point in time
- Not necessary to detach volume to do snapshot, but recommended
- Can copy snapshots across AZ or Region

US-EAST-1A US-EAST-1B

EBS Snapshot

snapshot restore

EBS EBS

(50 GB) (50 GB)


### EBS Snapshots Features

EBS Snapshot

EBS Snapshot

Archive

- EBS Snapshot Archive
- Move a Snapshot to an ”archive tier” that is

archive

75% cheaper

- Takes within 24 to 72 hours for restoring the

archive

- Recycle Bin for EBS Snapshots EBS Snapshot Recycle Bin
- Setup rules to retain deleted snapshots so you

can recover them after an accidental deletion delete

- Specify retention (from 1 day to 1 year)
- Fast Snapshot Restore (FSR)
- Force full initialization of snapshot to have no

latency on the first use ($$$)


### AMI Over view

- AMI = Amazon Machine Image
- AMI are a customization of an EC2 instance
- You add your own software, configuration, operating system, monitoring…
- Faster boot / configuration time because all your software is pre-packaged
- AMI are built for a specific region (and can be copied across regions)
- You can launch EC2 instances from:
- A Public AMI: AWS provided
- Your own AMI: you make and maintain them yourself
- An AWS Marketplace AMI: an AMI someone else made (and potentially sells)

### AMI Process (from an EC2 instance)

- Start an EC2 instance and customize it
- Stop the instance (for data integrity)
- Build an AMI – this will also create EBS snapshots
- Launch instances from other AMIs

Custom AMI

US-EAST-1A US-EAST-1B

Launch

Create AMI from AMI


### EC2 Instance Store

- EBS volumes are network drives with good but “limited” performance
- If you need a high-performance hardware disk, use EC2 Instance Store
- Better I/O performance
- EC2 Instance Store lose their storage if they’re stopped (ephemeral)
- Good for buffer / cache / scratch data / temporary content
- Risk of data loss if hardware fails
- Backups and Replication are your responsibility

### Local EC2 Instance Store

Very high IOPS


### EBS Volume Types

- EBS Volumes come in 6 types
- gp2 / gp3 (SSD): General purpose SSD volume that balances price and performance for

a wide variety of workloads

- io1 / io2 Block Express (SSD): Highest-performance SSD volume for mission-critical

low-latency or high-throughput workloads

- st1 (HDD): Low cost HDD volume designed for frequently accessed, throughput-

intensive workloads

- sc1 (HDD): Lowest cost HDD volume designed for less frequently accessed workloads
- EBS Volumes are characterized in Size | Throughput | IOPS (I/O Ops Per Sec)
- When in doubt always consult the AWS documentation – it’s good!
- Only gp2/gp3 and io1/io2 Block Express can be used as boot volumes

### EBS Volume Types Use cases

General Purpose SSD

- Cost effective storage, low-latency
- System boot volumes, Virtual desktops, Development and test environments
- 1 GiB - 16 TiB
- gp3:
- Baseline of 3,000 IOPS and throughput of 125 MiB/s
- Can increase IOPS up to 16,000 and throughput up to 1000 MiB/s independently
- gp2:
- Small gp2 volumes can burst IOPS to 3,000
- Size of the volume and IOPS are linked, max IOPS is 16,000
- 3 IOPS per GiB, means at 5,334 GiB we are at the max IOPS

### EBS Volume Types Use cases

Provisioned IOPS (PIOPS) SSD

- Critical business applications with sustained IOPS performance
- Or applications that need more than 16,000 IOPS
- Great for databases workloads (sensitive to storage perf and consistency)
- io1 (4 GiB - 16 TiB):
- Max PIOPS: 64,000 for Nitro EC2 instances & 32,000 for other
- Can increase PIOPS independently from storage size
- io2 Block Express (4 GiB – 64 TiB):
- Sub-millisecond latency
- Max PIOPS: 256,000 with an IOPS:GiB ratio of 1,000:1
- Supports EBS Multi-attach

### EBS Volume Types Use cases

Hard Disk Drives (HDD)

- Cannot be a boot volume
- 125 GiB to 16 TiB
- Throughput Optimized HDD (st1)
- Big Data, Data Warehouses, Log Processing
- Max throughput 500 MiB/s – max IOPS 500
- Cold HDD (sc1):
- For data that is infrequently accessed
- Scenarios where lowest cost is important
- Max throughput 250 MiB/s – max IOPS 250

### EBS – Volume Types Summary

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#solid-state-drives


### EBS Multi-Attach – io1/io2 family

- Attach the same EBS volume to multiple EC2

instances in the same AZ Availability Zone 1

- Each instance has full read & write permissions

to the high-performance volume

- Use case:
- Achieve higher application availability in clustered

Linux applications (ex: Teradata)

- Applications must manage concurrent write

operations

- Up to 16 EC2 Instances at a time
- Must use a file system that’s cluster-aware (not

io2 volume with Multi-Attach

XFS, EXT4, etc…)


### EBS Encr yption

- When you create an encrypted EBS volume, you get the following:
- Data at rest is encrypted inside the volume
- All the data in flight moving between the instance and the volume is encrypted
- All snapshots are encrypted
- All volumes created from the snapshot
- Encryption and decryption are handled transparently (you have nothing to
- Encryption has a minimal impact on latency
- EBS Encryption leverages keys from KMS (AES-256)
- Copying an unencrypted snapshot allows encryption
- Snapshots of encrypted volumes are encrypted

### Encr yption: encr ypt an unencr ypted EBS volume

- Create an EBS snapshot of the volume
- Encrypt the EBS snapshot ( using copy )
- Create new ebs volume from the snapshot ( the volume will also be

encrypted )

- Now you can attach the encrypted volume to the original instance

### Amazon EFS – Elastic File System

- Managed NFS (network file system) that can be mounted on many EC2
- EFS works with EC2 instances in multi-AZ
- Highly available, scalable, expensive (3x gp2), pay per use

us-east-1a us-east-1b us-east-1c

EC2 Instances EC2 Instances EC2 Instances

Security Group

EFS FileSystem


### Amazon EFS – Elastic File System

- Use cases: content management, web serving, data sharing, Wordpress
- Uses NFSv4.1 protocol
- Uses security group to control access to EFS
- Compatible with Linux based AMI (not Windows)
- Encryption at rest using KMS
- POSIX file system (~Linux) that has a standard file API
- File system scales automatically, pay-per-use, no capacity planning!

### EFS – Performance & Storage Classes

- EFS Scale
- 1000s of concurrent NFS clients, 10 GB+ /s throughput
- Grow to Petabyte-scale network file system, automatically
- Performance Mode (set at EFS creation time)
- General Purpose (default) – latency-sensitive use cases (web server, CMS, etc…)
- Max I/O – higher latency, throughput, highly parallel (big data, media processing)
- Throughput Mode
- Bursting – 1 TB = 50MiB/s + burst of up to 100MiB/s
- Provisioned – set your throughput regardless of storage size, ex: 1 GiB/s for 1 TB storage
- Elastic – automatically scales throughput up or down based on your workloads
- Up to 3GiB/s for reads and 1GiB/s for writes
- Used for unpredictable workloads

### EFS – Storage Classes

- Storage Tiers (lifecycle management feature – move

file after N days)

- Standard: for frequently accessed files
- Infrequent access (EFS-IA): cost to retrieve files, lower price

to store.

- Archive: rarely accessed data (few times each year), 50%

cheaper no access

for 60 days

- Implement lifecycle policies to move files between storage

tiers

EFS Standard

- Availability and durability move

Lifecycle Policy

- Standard: Multi-AZ, great for prod
- One Zone: One AZ, great for dev, backup enabled by

default, compatible with IA (EFS One Zone-IA)

- Over 90% in cost savings EFS IA

Amazon EFS File System


### EBS vs EFS – Elastic Block Storage

- EBS volumes…

Availability Zone 1 Availability Zone 2

- one instance (except multi-attach io1/io2)
- are locked at the Availability Zone (AZ) level
- gp2: IO increases if the disk size increases
- gp3 & io1: can increase IO independently
- To migrate an EBS volume across AZ
- Take a snapshot
- Restore the snapshot to another AZ EBS EBS
- EBS backups use IO and you shouldn’t run

them while your application is handling a lot

of traffic

snapshot restore

- Root EBS Volumes of instances get

terminated by default if the EC2 instance

gets terminated. (you can disable that)

EBS Snapshot


### EBS vs EFS – Elastic File System

- Mounting 100s of instances across AZ

Availability Zone 1 Availability Zone 2

- EFS share website files (WordPress)

Linux Linux

- Only for Linux Instances (POSIX)
- EFS has a higher price point than EBS

EFS EFS

- Can leverage Storage Tiers for cost savings

Mount Mount

Target Target

- Remember: EFS vs EBS vs Instance Store

---

## High Availability & Scalability


### Scalability & High Availability

- Scalability means that an application / system can handle greater loads

by adapting.

- There are two kinds of scalability:
- Vertical Scalability
- Horizontal Scalability (= elasticity)
- Scalability is linked but different to High Availability
- Let’s deep dive into the distinction, using a call center as an example

### Ver tical Scalability

- Vertically scalability means increasing the size

of the instance

- For example, your application runs on a

t2.micro

- Scaling that application vertically means

running it on a t2.large

- Vertical scalability is very common for non

distributed systems, such as a database.

- RDS, ElastiCache are services that can scale

vertically.

- There’s usually a limit to how much you can

vertically scale (hardware limit)

junior operator senior operator


### Horizontal Scalability

operator operator operator

- Horizontal Scalability means increasing the

number of instances / systems for your

application

- Horizontal scaling implies distributed systems.
- This is very common for web applications /

modern applications

- It’s easy to horizontally scale thanks the cloud

offerings such as Amazon EC2

operator operator operator


### High Availability

first building in New York

- High Availability usually goes hand in

hand with horizontal scaling

- High availability means running your

application / system in at least 2 data

centers (== Availability Zones)

- The goal of high availability is to survive

a data center loss

second building in San Francisco

- The high availability can be passive (for

RDS Multi AZ for example)

- The high availability can be active (for

horizontal scaling)


### High Availability & Scalability For EC2

- Vertical Scaling: Increase instance size (= scale up / down)
- From: t2.nano - 0.5G of RAM, 1 vCPU
- To: u-12tb1.metal – 12.3 TB of RAM, 448 vCPUs
- Horizontal Scaling: Increase number of instances (= scale out / in)
- Auto Scaling Group
- Load Balancer
- High Availability: Run instances for the same application across multi AZ
- Auto Scaling Group multi AZ
- Load Balancer multi AZ

### What is load balancing?

- Load Balances are servers that forward traffic to multiple

servers (e.g., EC2 instances) downstream

Elastic Load Balancer

EC2 Instance

EC2 Instance

EC2 Instance


### Why use a load balancer?

- Spread load across multiple downstream instances
- Expose a single point of access (DNS) to your application
- Seamlessly handle failures of downstream instances
- Do regular health checks to your instances
- Provide SSL termination (HTTPS) for your websites
- Enforce stickiness with cookies
- High availability across zones
- Separate public traffic from private traffic

### Why use an Elastic Load Balancer?

- An Elastic Load Balancer is a managed load balancer
- AWS guarantees that it will be working
- AWS takes care of upgrades, maintenance, high availability
- AWS provides only a few configuration knobs
- It costs less to setup your own load balancer but it will be a lot more effort

on your end

- It is integrated with many AWS offerings / services
- EC2, EC2 Auto Scaling Groups, Amazon ECS
- AWS Certificate Manager (ACM), CloudWatch
- Route 53, AWS WAF, AWS Global Accelerator

### Health Checks

- Health Checks are crucial for Load Balancers
- They enable the load balancer to know if instances it forwards traffic to

are available to reply to requests

- The health check is done on a port and a route (/health is common)
- If the response is not 200 (OK), then the instance is unhealthy

Protocol: HTTP

Port: 4567

Health Checks

Endpoint: /health

EC2 Instance

Elastic Load Balancer


### Types of load balancer on AWS

- AWS has 4 kinds of managed Load Balancers
- Classic Load Balancer (v1 - old generation) – 2009 – CLB
- HTTP, HTTPS, TCP, SSL (secure TCP)
- Application Load Balancer (v2 - new generation) – 2016 – ALB
- HTTP, HTTPS, WebSocket
- Network Load Balancer (v2 - new generation) – 2017 – NLB
- TCP, TLS (secure TCP), UDP
- Gateway Load Balancer – 2020 – GWLB
- Operates at layer 3 (Network layer) – IP Protocol
- Overall, it is recommended to use the newer generation load balancers as they

provide more features

- Some load balancers can be setup as internal (private) or external (public) ELBs

### Load Balancer Security Groups

LOAD BALANCER

HTTPS / HTTP HTTP Restricted

From anywhere to Load balancer

Users EC2

Load Balancer Security Group:

Application Security Group: Allow traffic only from Load Balancer


### Classic Load Balancers (v1)

- Supports TCP (Layer 4), HTTP &

HTTPS (Layer 7)

listener internal

- Health checks are TCP or HTTP

based

- Fixed hostname

Client CLB EC2

XXX.region.elb.amazonaws.com


### Application Load Balancer (v2)

- Application load balancers is Layer 7 (HTTP)
- Load balancing to multiple HTTP applications across machines

(target groups)

- Load balancing to multiple applications on the same machine

(ex: containers)

- Support for HTTP/2 and WebSocket
- Support redirects (from HTTP to HTTPS for example)

### Application Load Balancer (v2)

- Routing tables to different target groups:
- Routing based on path in URL (example.com/users & example.com/posts)
- Routing based on hostname in URL (one.example.com & other.example.com)
- Routing based on Query String, Headers

(example.com/users?id=123&order=false)

- ALB are a great fit for micro services & container-based application

(example: Docker & Amazon ECS)

- Has a port mapping feature to redirect to a dynamic port in ECS
- In comparison, we’d need multiple Classic Load Balancer per application

### puorG

tegraT

sresU

noitacilppa

Application Load Balancer (v2)

HTTP Based Traffic

WWW Route /user HTTP

External

Application

Load Balancer

(v2) puorG

tegraT

hcraeS

noitacilppa

WWW Route /search HTTP

kcehC

htlaeH

kcehC

htlaeH


### Application Load Balancer (v2)

Target Groups

- EC2 instances (can be managed by an Auto Scaling Group) – HTTP
- ECS tasks (managed by ECS itself) – HTTP
- Lambda functions – HTTP request is translated into a JSON event
- IP Addresses – must be private IPs
- ALB can route to multiple target groups
- Health checks are at the target group level

### Application Load Balancer (v2)

Quer y Strings/Parameters Routing

Target Group 1

?Platform=Mobile AWS – EC2 based

External

Requests

WWW Application

Load Balancer

(v2)

Target Group 2

On-premises – Private IP routing

?Platform=Desktop


### Application Load Balancer (v2)

Good to Know

- Fixed hostname (XXX.region.elb.amazonaws.com)
- The application servers don’t see the IP of the client directly
- The true IP of the client is inserted in the header X-Forwarded-For
- We can also get Port (X-Forwarded-Port) and proto (X-Forwarded-Proto)

Load Balancer IP

Client IP

(Private IP)

12.34.56.78 Instance

Connection termination


### Network Load Balancer (v2)

- Network load balancers (Layer 4) allow to:
- Forward TCP & UDP traffic to your instances
- Handle millions of request per seconds
- Ultra-low latency
- NLB has one static IP per AZ, and supports assigning Elastic IP

(helpful for whitelisting specific IP)

- NLB are used for extreme performance, TCP or UDP traffic

### Network Load Balancer (v2)

TCP (Layer 4) Based Traffic

puorG

tegraT

sresU

noitacilppa

WWW TCP + Rules TCP

External

Network Load

Balancer (v2)

puorG

tegraT

hcraeS

noitacilppa

WWW TCP + Rules HTTP

kcehC

htlaeH

kcehC

htlaeH


### Network Load Balancer – Target Groups

- EC2 instances
- IP Addresses – must be private IPs
- Application Load Balancer
- Health Checks support the TCP, HTTP and HTTPS Protocols

Network Network Network

Load Balancer Load Balancer Load Balancer

i-1234567890abcdef0 i-1234567890abcdef0 192.168.1.118 10.0.4.21

Target Group Target Group Target Group

(EC2 Instances) (IP Addresses) (Application Load Balancer)


### Gateway Load Balancer

- Deploy, scale, and manage a fleet of 3rd party Route

Table

network virtual appliances in AWS

- Example: Firewalls, Intrusion Detection and

Users Application

Prevention Systems, Deep Packet Inspection

(source) (destination)

Systems, payload manipulation, …

traffic traffic

- Operates at Layer 3 (Network Layer) – IP

Gateway

Packets

Load Balancer

- Combines the following functions:
- Transparent Network Gateway – single entry/exit

for all traffic

- Load Balancer – distributes traffic to your virtual

Target Group

appliances

- Uses the GENEVE protocol on port 6081

3rd Party Security

Virtual Appliances


### Gateway Load Balancer – Target Groups

- EC2 instances
- IP Addresses – must be private IPs

Gateway Gateway

Load Balancer Load Balancer

i-1234567890abcdef0 i-1234567890abcdef0 192.168.1.118 10.0.4.21

Target Group Target Group

(EC2 Instances) (IP Addresses)


### Sticky Sessions (Session Affinity)

- It is possible to implement stickiness so that the

same client is always redirected to the same

Client 1 Client 2 Client 3

instance behind a load balancer

- This works for Classic Load Balancer, Application

Load Balancer, and Network Load Balancer

- For both CLB & ALB, the “cookie” used for

stickiness has an expiration date you control

- Use case: make sure the user doesn’t lose his

session data

- Enabling stickiness may bring imbalance to the

load over the backend EC2 instances

EC2 Instance EC2 Instance


### Sticky Sessions – Cookie Names

- Application-based Cookies
- Custom cookie
- Generated by the target
- Can include any custom attributes required by the application
- Cookie name must be specified individually for each target group
- Don’t use AWSALB, AWSALBAPP, or AWSALBTG (reserved for use by the ELB)
- Application cookie
- Generated by the load balancer
- Cookie name is AWSALBAPP
- Duration-based Cookies
- Cookie generated by the load balancer
- Cookie name is AWSALB for ALB, AWSELB for CLB

### Cross-Zone Load Balancing

With Cross Zone Load Balancing: Without Cross Zone Load Balancing:

each load balancer instance distributes evenly Requests are distributed in the instances of the

across all registered instances in all AZ node of the Elastic Load Balancer

50 50

50 50

10 10 10 10 6.25 6.25 6.25 6.25

10 10 25 25

10 10 10 10 6.25 6.25 6.25 6.25

Availability Zone 1 Availability Zone 2 Availability Zone 1 Availability Zone 2


### Cross-Zone Load Balancing

- Application Load Balancer
- Enabled by default (can be disabled at the Target Group level)
- No charges for inter AZ data
- Network Load Balancer & Gateway Load Balancer
- Disabled by default
- You pay charges ($) for inter AZ data if enabled
- Classic Load Balancer
- Disabled by default
- No charges for inter AZ data if enabled

### SSL/TLS - Basics

- An SSL Certificate allows traffic between your clients and your load balancer

to be encrypted in transit (in-flight encryption)

- SSL refers to Secure Sockets Layer, used to encrypt connections
- TLS refers to Transport Layer Security, which is a newer version
- Nowadays, TLS certificates are mainly used, but people still refer as SSL
- Public SSL certificates are issued by Certificate Authorities (CA)
- Comodo, Symantec, GoDaddy, GlobalSign, Digicert, Letsencrypt, etc…
- SSL certificates have an expiration date (you set) and must be renewed

### Load Balancer - SSL Cer tificates

LOAD BALANCER

HTTPS (encrypted) HTTP

Over www Over private VPC

Users

Instance

- The load balancer uses an X.509 certificate (SSL/TLS server certificate)
- You can manage certificates using ACM (AWS Certificate Manager)
- You can create upload your own certificates alternatively
- HTTPS listener:
- You must specify a default certificate
- You can add an optional list of certs to support multiple domains
- Clients can use SNI (Server Name Indication) to specify the hostname they reach
- Ability to specify a security policy to support older versions of SSL / TLS (legacy clients)

### SSL – Ser ver Name Indication (SNI)

- SNI solves the problem of loading multiple SSL

Target group for

certificates onto one web server (to serve www.mycorp.com

multiple websites)

- It’s a “newer” protocol, and requires the client

to indicate the hostname of the target server

in the initial SSL handshake Target group for

Domain1.example.com

- The server will then find the correct I would like

www.mycorp.com

certificate, or return the default one

Client ALB

Note:

SSL Cert:

- Only works for ALB & NLB (newer Domain1.example.com

Use the correct

generation), CloudFront SSL cert

- Does not work for CLB (older gen) SSL Cert:

www.mycorp.com


### Elastic Load Balancers – SSL Cer tificates

- Classic Load Balancer (v1)
- Support only one SSL certificate
- Must use multiple CLB for multiple hostname with multiple SSL certificates
- Application Load Balancer (v2)
- Supports multiple listeners with multiple SSL certificates
- Uses Server Name Indication (SNI) to make it work
- Network Load Balancer (v2)
- Supports multiple listeners with multiple SSL certificates
- Uses Server Name Indication (SNI) to make it work

### Connection Draining

- Feature naming
- Connection Draining – for CLB waiting for existing
- Deregistration Delay – for ALB & NLB connections to complete

EC2 Instance

- Time to complete “in-flight requests” while the

DRAINING

instance is de-registering or unhealthy

- Stops sending new requests to the EC2

instance which is de-registering

EC2 Instance

Users

- Between 1 to 3600 seconds (default: 300

seconds)

- Can be disabled (set value to 0)

new connections

established to all other instances

- Set to a low value if your requests are short

EC2 Instance


### What’s an Auto Scaling Group?

- In real-life, the load on your websites and application can change
- In the cloud, you can create and get rid of servers very quickly
- The goal of an Auto Scaling Group (ASG) is to:
- Scale out (add EC2 instances) to match an increased load
- Scale in (remove EC2 instances) to match a decreased load
- Ensure we have a minimum and a maximum number of EC2 instances running
- Automatically register new instances to a load balancer
- Re-create an EC2 instance in case a previous one is terminated (ex: if unhealthy)
- ASG are free (you only pay for the underlying EC2 instances)

### Auto Scaling Group in AWS

Auto Scaling Group

EC2 EC2 EC2 EC2 EC2 EC2 EC2

Instance Instance Instance Instance Instance Instance Instance

Minimum Capacity Scale Out as Needed

Desired Capacity

Maximum Capacity


### Auto Scaling Group in AWS With Load Balancer

Users

Elastic Load Balancer

ELB can check the health of your EC2 instances!

Auto Scaling Group

EC2 EC2 EC2 EC2 EC2 EC2 EC2

Instance Instance Instance Instance Instance Instance Instance


|  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |


### Auto Scaling Group Attributes

- A Launch Template (older “Launch Configurations” are deprecated)
- AMI + Instance Type
- EC2 User Data ASG Launch Template
- EBS Volumes
- Security Groups
- SSH Key Pair AMI Instance EBS Volumes

Type

- IAM Roles for your EC2 Instances
- Network + Subnets Information Security

Groups SSH Key Pair IAM Role

- Load Balancer Information
- Min Size / Max Size / Initial Capacity
- Scaling Policies

VPC + Subnets Load

Balancer


### Auto Scaling - CloudWatch Alarms & Scaling

- It is possible to scale an ASG based on CloudWatch alarms
- An alarm monitors a metric (such as Average CPU, or a custom metric)
- Metrics such as Average CPU are computed for the overall ASG instances
- Based on the alarm:
- We can create scale-out policies (increase the number of instances)
- We can create scale-in policies (decrease the number of instances)

Auto Scaling Group

trigger Scaling

EC2 EC2 EC2 EC2 EC2

Instance Instance Instance Instance Instance

CloudWatch

Alarm


### Auto Scaling Groups – Scaling Policies

- Dynamic Scaling
- Target Tracking Scaling
- Simple to set-up
- Example: I want the average ASG CPU to stay at around 40%
- Simple / Step Scaling
- When a CloudWatch alarm is triggered (example CPU > 70%), then add 2 units
- When a CloudWatch alarm is triggered (example CPU < 30%), then remove 1
- Scheduled Scaling
- Anticipate a scaling based on known usage patterns
- Example: increase the min capacity to 10 at 5 pm on Fridays

### Auto Scaling Groups – Scaling Policies

- Predictive scaling: continuously forecast load and schedule scaling ahead

### Good metrics to scale on

Users

- CPUUtilization: Average CPU

utilization across your instances

- RequestCountPerTarget: to make sure

the number of requests per EC2

Application

Load Balancer

instances is stable

- Average Network In / Out (if you’re RequestCountPerTarget

Target Value: 3

application is network bound)

- Any custom metric (that you push

using CloudWatch)

Auto Scaling group


### Auto Scaling Groups - Scaling Cooldowns

- After a scaling activity happens, you are in

Scaling Action

the cooldown period (default 300 Occurs

seconds)

- During the cooldown period, the ASG will

not launch or terminate additional

Default

instances (to allow for metrics to stabilize) Launch or

Cooldown

Teminate Instance

in effect?

- Advice: Use a ready-to-use AMI to reduce

configuration time in order to be serving

request fasters and reduce the cooldown

Ignore Action

period


---

## RDS, Aurora & ElastiCache


### Amazon RDS Over view

- RDS stands for Relational Database Service
- It’s a managed DB service for DB use SQL as a query language.
- It allows you to create databases in the cloud that are managed by AWS
- Postgres
- MySQL
- MariaDB
- Oracle
- Microsoft SQL Server
- IBM DB2
- Aurora (AWS Proprietary database)

### Advantage over using RDS versus deploying

DB on EC2

- RDS is a managed service:
- Automated provisioning, OS patching
- Continuous backups and restore to specific timestamp (Point in Time Restore)!
- Monitoring dashboards
- Read replicas for improved read performance
- Multi AZ setup for DR (Disaster Recovery)
- Maintenance windows for upgrades
- Scaling capability (vertical and horizontal)
- Storage backed by EBS
- BUT you can’t SSH into your instances

### RDS – Storage Auto Scaling

- Helps you increase storage on your RDS DB instance

dynamically

- When RDS detects you are running out of free database

storage, it scales automatically

- Avoid manually scaling your database storage

Application

- You have to set Maximum Storage Threshold (maximum limit

for DB storage)

- Automatically modify storage if:

Read/Write

- Free storage is less than 10% of allocated storage
- Low-storage lasts at least 5 minutes
- 6 hours have passed since last modification
- Useful for applications with unpredictable workloads
- Supports all RDS database engines

Storage


### RDS Read Replicas for read scalability

- Up to 15 Read Replicas
- Within AZ, Cross AZ or

Application

Cross Region

- Replication is ASYNC,

so reads are eventually

consistent writes

reads reads reads

- Replicas can be

promoted to their own

- Applications must

update the connection

string to leverage read ASYNC ASYNC

replicas replication replication

RDS DB RDS DB

RDS DB

instance read instance read

instance

replica replica


### RDS Read Replicas – Use Cases

- You have a production database

that is taking on normal load Production Reporting

Application Application

- You want to run a reporting

application to run some analytics

- You create a Read Replica to run

rreeaaddss

reads

the new workload there

- The production application is

unaffected

- Read replicas are used for SELECT

(=read) only kind of statements

ASYNC

(not INSERT, UPDATE, DELETE)

replication

RDS DB

RDS DB

instance read

instance

replica


### RDS Read Replicas – Network Cost

- In AWS there’s a network cost when data goes from one AZ to another
- For RDS Read Replicas within the same region, you don’t pay that fee

Same Region / Different AZ Region/AZ Region/AZ

us-east-1a us-east-1b us-east-1a eu-west-1b

ASYNC ASYNC

Replication Replication

RDS DB RDS DB

RDS DB RDS DB

Same Region instance read Cross-Region instance read

instance instance

Free replica $$$ replica


### RDS Multi AZ (Disaster Recover y)

- SYNC replication

Application

- One DNS name – automatic app

failover to standby

- Increase availability writes

reads

- Failover in case of loss of AZ, loss of

network, instance or storage failure

One DNS name – automatic failover

- No manual intervention in apps
- Not used for scaling
- Note: The Read Replicas be setup as

SYNC

Multi AZ for Disaster Recovery (DR)

replication

RDS DB

RDS Master DB

instance standby

instance (AZ A)

(AZ B)


### RDS – From Single-AZ to Multi-AZ

- Zero downtime operation (no

RDS DB Standby DB

need to stop the DB)

instance

- Just click on “modify” for the

database

- The following happens internally:

SYNC

Replication

- A snapshot is taken
- A new DB is restored from the

snapshot in a new AZ

restore

snapshot

- Synchronization is established

between the two databases

DB snapshot


### RDS Custom

User

- Managed Oracle and Microsoft SQL Server Database with OS and

database customization

apply

- RDS: Automates setup, operation, and scaling of database in AWS

cstomizations

- Custom: access to the underlying database and OS so you can
- Configure settings
- Install patches

EC2 Instance

- Enable native features
- Access the underlying EC2 Instance using SSH or SSM Session Manager Automation

Mode disabled

- De-activate Automation Mode to perform your customization,

better to take a DB snapshot before

- RDS vs. RDS Custom
- RDS: entire database and the OS to be managed by AWS
- RDS Custom: full admin access to the underlying OS and the database

### Amazon Aurora

- Aurora is a proprietary technology from AWS (not open sourced)
- Postgres and MySQL are both supported as Aurora DB (that means your

drivers will work as if Aurora was a Postgres or MySQL database)

- Aurora is “AWS cloud optimized” and claims 5x performance improvement

over MySQL on RDS, over 3x the performance of Postgres on RDS

- Aurora storage automatically grows in increments of 10GB, up to 256 TB.
- Aurora can have up to 15 replicas and the replication process is faster than

MySQL (sub 10 ms replica lag)

- Failover in Aurora is instantaneous. It’s HA (High Availability) native.
- Aurora costs more than RDS (20% more) – but is more efficient

### Aurora High Availability and Read Scaling

- 6 copies of your data across 3 AZ:

AZ 1 AZ 2 AZ 3

- 4 copies out of 6 needed for writes
- 3 copies out of 6 need for reads
- Self healing with peer-to-peer replication
- Storage is striped across 100s of volumes

R R R R R

- One Aurora Instance takes writes (master)
- Automated failover for master in less than Shared storage Volume

Replication + Self Healing + Auto Expanding

30 seconds

- Master + up to 15 Aurora Read Replicas

serve reads

- Support for Cross Region Replication

| Sha Replication + | red storage Volum Self Healing + Aut | e o Expanding |
| --- | --- | --- |


### Aurora DB Cluster

client

Writer Endpoint Reader Endpoint

Pointing to the master Connection Load Balancing

Auto Scaling

W R R R R R

Shared storage Volume

Auto Expanding from 10G to 256 TB


### Features of Aurora

- Automatic fail-over
- Backup and Recovery
- Isolation and security
- Industry compliance
- Push-button scaling
- Automated Patching with Zero Downtime
- Advanced Monitoring
- Routine Maintenance
- Backtrack: restore data at any point of time without using backups

### Aurora Replicas - Auto Scaling

Client

Many Requests

Writer Endpoint Reader Endpoint Endpoint Extended

CPU CPU

Usage Usage

Replicas Auto Scaling

R R R R

Shared Storage Volume


| Reader Endpoint | Endpoint Extended |
| --- | --- |


### Aurora – Custom Endpoints

- Define a subset of Aurora Instances as a Custom Endpoint
- Example: Run analytical queries on specific replicas
- The Reader Endpoint is generally not used after defining Custom Endpoints

Analytical Queries

Queries

Client

Writer Endpoint Reader Endpoint Custom Endpoint

db.r3.large db.r3.large db.r5.2xlarge db.r5.2xlarge

R R R R

Shared Storage Volume


### Aurora Ser verless

- Automated database Client

instantiation and auto-

scaling based on actual

usage

- Good for infrequent, Proxy Fleet

(managed by Aurora)

intermittent or

unpredictable workloads

- No capacity planning

needed

- Pay per second, can be

more cost-effective

Shared storage Volume


### Global Aurora

us-east-1 - PRIMARY region

- Aurora Cross Region Read Replicas:
- Useful for disaster recovery
- Simple to put in place

Applications

- Aurora Global Database (recommended):

Read / Write

- 1 Primary Region (read / write)
- Up to 10 secondary (read-only) regions, replication lag is less replication

than 1 second

eu-west-1 - SECONDARY region

- Up to 16 Read Replicas per secondary region
- Helps for decreasing latency
- Promoting another region (for disaster recovery) has an RTO

of < 1 minute

- Typical cross-region replication takes less than 1 second

Applications

Read Only


### Aurora Machine Learning

- Enables you to add ML-based predictions to

Application

your applications via SQL

SQL query query results

- Simple, optimized, and secure integration

(Recommended products?) (red shirt, blue …)

between Aurora and AWS ML services

Amazon Aurora

- Supported services
- Amazon SageMaker (use with any ML model)

data

predictions

(user’s profile,

- Amazon Comprehend (for sentiment analysis) (red shirt,

shopping history, …)

blue pants, …)

- You don’t need to have ML experience
- Use cases: fraud detection, ads targeting,

sentiment analysis, product recommendations

Amazon Amazon

SageMaker Comprehend


### Babelfish for Aurora PostgreSQL

- Allows Aurora PostgreSQL to

Application Application

understand commands targeted for

SQL Server Client Driver PostgreSQL Driver

MS SQL Server

(e.g., T-SQL)

- Therefore Microsoft SQL Server

based applications can work on

T-SQL PL/pgSQL

Aurora PostgreSQL

- Requires no to little code changes

T-SQL

(using the same MS SQL Server client

driver)

Babelfish PostgreSQL

- The same applications can be used

after a migration of your database

migrate

(using AWS SCT and DMS)

Aurora PostgreSQL


### RDS Backups

- Automated backups:
- Daily full backup of the database (during the backup window)
- Transaction logs are backed-up by RDS every 5 minutes
- => ability to restore to any point in time (from oldest backup to 5 minutes ago)
- 1 to 35 days of retention, set 0 to disable automated backups
- Manual DB Snapshots
- Manually triggered by the user
- Retention of backup for as long as you want
- Trick: in a stopped RDS database, you will still pay for storage. If you plan on

stopping it for a long time, you should snapshot & restore instead


### Aurora Backups

- Automated backups
- 1 to 35 days (cannot be disabled)
- point-in-time recovery in that timeframe
- Manual DB Snapshots
- Manually triggered by the user
- Retention of backup for as long as you want

### RDS & Aurora Restore options

- Restoring a RDS / Aurora backup or a snapshot creates a new database
- Restoring MySQL RDS database from S3
- Create a backup of your on-premises database
- Store it on Amazon S3 (object storage)
- Restore the backup file onto a new RDS instance running MySQL
- Restoring MySQL Aurora cluster from S3
- Create a backup of your on-premises database using Percona XtraBackup
- Store the backup file on Amazon S3
- Restore the backup file onto a new Aurora cluster running MySQL

### Aurora Database Cloning

- Create a new Aurora DB Cluster from an

existing one

- Faster than snapshot & restore
- Uses copy-on-write protocol
- Initially, the new DB cluster uses the same data clone

volume as the original DB cluster (fast and efficient

- no copying is needed)
- When updates are made to the new DB cluster

data, then additional storage is allocated and data is Production Aurora Staging Aurora

copied to be separated

- Very fast & cost-effective
- Useful to create a “staging” database from a

“production” database without impacting the

production database


### RDS & Aurora Security

- At-rest encryption:
- Database master & replicas encryption using AWS KMS – must be defined as launch time
- If the master is not encrypted, the read replicas cannot be encrypted
- To encrypt an un-encrypted database, go through a DB snapshot & restore as encrypted
- In-flight encryption: TLS-ready by default, use the AWS TLS root certificates client-side
- IAM Authentication: IAM roles to connect to your database (instead of username/pw)
- Security Groups: Control Network access to your RDS / Aurora DB
- No SSH available except on RDS Custom
- Audit Logs can be enabled and sent to CloudWatch Logs for longer retention

### Amazon RDS Proxy

- Fully managed database proxy for RDS

Lambda functions

- Allows apps to pool and share DB connections

established with the database

- Improving database efficiency by reducing the stress IAM

on database resources (e.g., CPU, RAM) and Authentication

minimize open connections (and timeouts)

- Serverless, autoscaling, highly available (multi-AZ)
- Reduced RDS & Aurora failover time by up 66% Private subnet
- Supports RDS (MySQL, PostgreSQL, MariaDB, MS

RDS Proxy

SQL Server) and Aurora (MySQL, PostgreSQL)

- No code changes required for most apps
- Enforce IAM Authentication for DB, and securely

store credentials in AWS Secrets Manager

- RDS Proxy is never publicly accessible (must be

RDS DB

accessed from VPC)

Instance


### Amazon ElastiCache Over view

- The same way RDS is to get managed Relational Databases…
- ElastiCache is to get managed Redis or Memcached
- Caches are in-memory databases with really high performance, low

latency

- Helps reduce load off of databases for read intensive workloads
- Helps make your application stateless
- AWS takes care of OS maintenance / patching, optimizations, setup,

configuration, monitoring, failure recovery and backups

- Using ElastiCache involves heavy application code changes

### ElastiCache

Solution Architecture - DB Cache

- Applications queries

Amazon

ElastiCache, if not

ElastiCache

available, get from RDS

Cache hit

and store in ElastiCache.

- Helps relieve load in RDS

Cache miss

- Cache must have an

application

Read from DB

invalidation strategy to

make sure only the most

current data is used in

Write to cache Amazon

there.


| Amazon ElastiCache Read from DB |
| --- |
|  |
| e |


### ElastiCache

Solution Architecture – User Session Store

- User logs into any of the

Write session

application

application

- The application writes

Amazon

the session data into

ElastiCache

ElastiCache

Retrieve session

- The user hits another

application

instance of our

application

- The instance retrieves the

data and the user is

application

already logged in

resU


### ElastiCache – Redis vs Memcached

REDIS MEMCACHED

- Multi AZ with Auto-Failover
- Multi-node for partitioning of
- Read Replicas to scale reads and data (sharding)

have high availability

- No high availability (replication)
- Data Durability using AOF
- Non persistent

persistence

- Backup and restore (Serverless)
- Backup and restore features
- Supports Sets and Sorted Sets • Multi-threaded architecture

Replication

sharding


### ElastiCache – Cache Security

- ElastiCache supports IAM Authentication for Redis

EC2 Security group

- IAM policies on ElastiCache are only used for

EC2 Client

AWS API-level security

- Redis AUTH
- You can set a “password/token” when you create a

SSL encryption

Redis cluster Redis AUTH

- This is an extra level of security for your cache (on top

of security groups)

Redis Security group

- Support SSL in flight encryption
- Memcached
- Supports SASL-based authentication (advanced)

### Patterns for ElastiCache

- Lazy Loading: all the read data is

cached, data can become stale in

Amazon

cache

ElastiCache

- Write Through: Adds or update

data in the cache when written

Cache hit

to a DB (no stale data)

- Session Store: store temporary

session data in a cache (using

TTL features)

Cache miss

application

Read from DB

- Quote: There are only two hard

things in Computer Science: cache

invalidation and naming things

Write to cache Amazon

Lazy Loading illustrated


| Amazon ElastiCache Read from DB |
| --- |
|  |
| e |


### ElastiCache – Redis Use Case

- Gaming Leaderboards are computationally complex
- Redis Sorted sets guarantee both uniqueness and element ordering
- Each time a new element added, it’s ranked in real time, then added in

correct order

ElastiCache

for Redis

ElastiCache

for Redis

Clients Real-time Leaderboard

ElastiCache

for Redis


| 2 |
| --- |
| 3 |


---

## Amazon Route 53


### What is DNS?

- Domain Name System which translates the human friendly hostnames

into the machine IP addresses

- www.google.com => 172.217.18.36
- DNS is the backbone of the Internet
- DNS uses hierarchical naming structure

example.com

www.example.com

api.example.com


### DNS Terminologies

- Domain Registrar: Amazon Route 53, GoDaddy, …
- DNS Records: A, AAAA, CNAME, NS, …
- Zone File: contains DNS records
- Name Server: resolves DNS queries (Authoritative or Non-Authoritative)
- Top Level Domain (TLD): .com, .us, .in, .gov, .org, …
- Second Level Domain (SLD): amazon.com, google.com, …

http://api.www.example.com.

Root

Protocol TLD

Sub Domain

FQDN (Fully Qualified Domain Name)


### How DNS Works

Web Server

(example.com)

(IP: 9.10.11.12)

Managed by ICANN

mple.co

Root DNS Server

1.2.3.4

example.com?

example.com?

Managed by IANA

9.10.11.12

(Branch of ICANN)

example.com NS 5.6.7.8

Web Browser

TLD DNS Server

You want to access Local DNS Server exam

(.com)

example.com exam

ple.com

Assigned and Managed by ple.com

your

d by

9.10.11.12

Managed by Domain Registrar

(e.g., Amazon Registrar, Inc.)

SLD DNS Server

(example.com)


### Amazon Route 53

- A highly available, scalable, fully

example.com?

managed and Authoritative DNS Amazon

Route 53

- Authoritative = the customer (you)

54.22.33.44

Client

can update the DNS records

- Route 53 is also a Domain Registrar
- Ability to check the health of your

resources

AWS Cloud

- The only AWS service which

Public IP

provides 100% availability SLA

54.22.33.44

- Why Route 53? 53 is a reference to EC2 Instance

the traditional DNS port


### Route 53 – Records

- How you want to route traffic for a domain
- Each record contains:
- Domain/subdomain Name – e.g., example.com
- Record Type – e.g., A or AAAA
- Value – e.g., 12.34.56.78
- Routing Policy – how Route 53 responds to queries
- TTL – amount of time the record cached at DNS Resolvers
- Route 53 supports the following DNS record types:
- (must know) A / AAAA / CNAME / NS
- (advanced) CAA / DS / MX / NAPTR / PTR / SOA / TXT / SPF / SRV

### Route 53 – Record Types

- A – maps a hostname to IPv4
- AAAA – maps a hostname to IPv6
- CNAME – maps a hostname to another hostname
- The target is a domain name which must have an A or AAAA record
- Can’t create a CNAME record for the top node of a DNS namespace (Zone

Apex)

- Example: you can’t create for example.com, but you can create for

www.example.com

- NS – Name Servers for the Hosted Zone
- Control how traffic is routed for a domain

### Route 53 – Hosted Zones

- A container for records that define how to route traffic to a domain and

its subdomains

- Public Hosted Zones – contains records that specify how to route

traffic on the Internet (public domain names)

application1.mypublicdomain.com

- Private Hosted Zones – contain records that specify how you route

traffic within one or more VPCs (private domain names)

application1.company.internal

- You pay $0.50 per month per hosted zone

### Route 53 – Public vs. Private Hosted Zones

example.com?

54.22.33.44

Private Hosted Zone

Client

Public Hosted Zone

.i 0

m 0 . DB Instance

VPC x 0

. (db.example.internal)

a (Private IP)

S3 Bucket Amazon EC2 Instance Application EC2 Instance EC2 Instance

CloudFront (Public IP) Load Balancer (webapp.example.internal) (api.example.internal)

(Private IP) (Private IP)

?lanretni.elpmaxe.bd

53.0.0.01

Public Hosted Zone Private Hosted Zone


### Route 53 – Records TTL (Time To Live)

- High TTL – e.g., 24 hr

s ib

a ff

rds mya

t com?

- Low TTL – e.g., 60 sec.

Amazon

A 12.34.56.78 Route 53

- More traffic on Route 53 ($$)

(with TTL)

- Records are outdated for less

time

Client

HTTP

Request

- Easy to change records

or HTTP

Response

- Except for Alias records, TTL

is mandatory for each DNS

record

Web Server


### CNAME vs Alias

- AWS Resources (Load Balancer, CloudFront...) expose an AWS hostname:
- lb1-1234.us-east-2.elb.amazonaws.com and you want myapp.mydomain.com
- CNAME:
- Points a hostname to any other hostname. (app.mydomain.com => blabla.anything.com)
- ONLY FOR NON ROOT DOMAIN (aka. something.mydomain.com)
- Alias:
- Points a hostname to an AWS Resource (app.mydomain.com => blabla.amazonaws.com)
- Works for ROOT DOMAIN and NON ROOT DOMAIN (aka mydomain.com)
- Free of charge
- Native health check

### Route 53 – Alias Records

Amazon

Route 53

- Maps a hostname to an AWS resource
- An extension to DNS functionality
- Automatically recognizes changes in the Alias Record (Enabled)

Record Name Type Value

resource’s IP addresses

example.com A MyALB-123456789.us-

east-

- Unlike CNAME, it can be used for the top node

1.elb.amazonaws.com

of a DNS namespace (Zone Apex), e.g.:

example.com

- Alias Record is always of type A/AAAA for

MyALB-123456789.us-east-1.elb.amazonaws.com

AWS resources (IPv4 / IPv6)

AWS-Managed

(IP Addresses might change)

- You can’t set the TTL

Application

Load Balancer


| Record Name | Type | Value |
| --- | --- | --- |
| example.com | A | MyALB-123456789.us- east- 1.elb.amazonaws.com |


### Route 53 – Alias Records Targets

- Elastic Load Balancers
- CloudFront Distributions

Elastic Amazon Amazon

- API Gateway

Load Balancer CloudFront API Gateway

- Elastic Beanstalk environments
- S3 Websites
- VPC Interface Endpoints Elastic Beanstalk S3 Websites VPC Interface

Endpoints

- Global Accelerator accelerator
- Route 53 record in the same hosted zone

Global Accelerator Route 53 Record

- You cannot set an ALIAS record for an EC2 DNS name (same Hosted Zone)

### Route 53 – Routing Policies

- Define how Route 53 responds to DNS queries
- Don’t get confused by the word “Routing”
- It’s not the same as Load balancer routing which routes the traffic
- DNS does not route any traffic, it only responds to the DNS queries
- Route 53 Supports the following Routing Policies
- Simple
- Weighted
- Failover
- Latency based
- Geolocation
- Multi-Value Answer
- Geoproximity (using Route 53 Traffic Flow feature)

### Routing Policies – Simple

Single Value

- Typically, route traffic to a single

foo.example.com

resource

A 11.22.33.44

- Can specify multiple values in the

Client

same record Amazon

Route 53

- If multiple values are returned, a

random one is chosen by the client

Multiple Value

- When Alias enabled, specify only

one AWS resource

foo.example.com

- Can’t be associated with Health

Checks

A 11.22.33.44

Client

A 55.66.77.88

Amazon

A 99.11.22.33

chooses

Route 53

a random value


### Routing Policies – Weighted

- Control the % of the requests that go to each

specific resource

- Assign each record a relative weight:

% Weight: 70

!"#$%& ()* + ,-".#(#. *".)*/

- 𝑡𝑟𝑎𝑓𝑓𝑖𝑐 (%) =

012 )( +33 &%" 4"#$%&, ()* +33 *".)*/,

- Weights don’t need to sum up to 100
- DNS records must have the same name and type
- Can be associated with Health Checks 20%
- Use cases: load balancing between regions, testing

new application versions…

Amazon Weight: 20

Route 53

- Assign a weight of 0 to a record to stop sending

traffic to a resource %

- If all records have weight of 0, then all records will

be returned equally

Weight: 10


### Routing Policies – Latency-based

- Redirect to the resource that

has the least latency close to us

- Super helpful when latency for

users is a priority

- Latency is based on traffic

between users and AWS

Regions

- Germany users may be

(us-east-1)

directed to the US (if that’s the

lowest latency)

(ap-southeast-1)

- Can be associated with Health

Checks (has a failover

capability)


### Route 53 – Health Checks

Amazon Route 53

DNS Record

- HTTP Health Checks are only for public

(latency, geoproximity, …)

resources

- Health Check => Automated DNS Failover:

Health Check Health Check

1. Health checks that monitor an endpoint

(application, server, other AWS resource)

2. Health checks that monitor other health

us-east-1 eu-west-1

checks (Calculated Health Checks)

3. Health checks that monitor CloudWatch

Alarms (full control !!) – e.g., throttles of

ALB ALB

DynamoDB, alarms on RDS, custom metrics,

… (helpful for private resources)

Auto Scaling group Auto Scaling group

- Health Checks are integrated with CW

metrics

EC2 Instance EC2 Instance


### Health Checks – Monitor an Endpoint

- About 15 global health checkers will check the Health Checker Health Checker Health Checker

endpoint health (us-east-1) (us-west-1) (sa-east-1)

- Healthy/Unhealthy Threshold – 3 (default) H
- Interval – 30 sec (can set to 10 sec – higher cost) P

/ 2 h r

0 e e

- Supported protocol: HTTP, HTTPS and TCP 0 a q

o h e

- If > 18% of health checkers report the endpoint is d s t

healthy, Route 53 considers it Healthy. Otherwise, it’s

Unhealthy

eu-west-1

- Ability to choose which locations you want Route 53 to Must allow incoming

use requests from Route 53

Health Checkers IP

- Health Checks pass only when the endpoint

address range

responds with the 2xx and 3xx status codes

- Health Checks can be setup to pass / fail based on

the text in the first 5120 bytes of the response

Auto Scaling group

- Configure you router/firewall to allow incoming

requests from Route 53 Health Checkers

EC2 Instance

https://ip-ranges.amazonaws.com/ip-ranges.json


### Route 53 – Calculated Health Checks

Amazon Route 53

- Combine the results of multiple Health

Checks into a single Health Check

Health Check

- You can use OR, AND, or NOT

(Parent)

- Can monitor up to 256 Child Health Checks
- Specify how many of the health checks need

to pass to make the parent pass

Health Check Health Check Health Check

- Usage: perform maintenance to your website

(Child) (Child) (Child)

without causing all health checks to fail

monitor monitor monitor

EC2 Instance EC2 Instance EC2 Instance


### Health Checks – Private Hosted Zones

- Route 53 health checkers are outside the
- They can’t access private endpoints

Private subnet

(private VPC or on-premises resource)

Health Checker

(us-east-1)

- You can create a CloudWatch Metric and

monitor

associate a CloudWatch Alarm, then

create a Health Check that checks the

monitor

alarm itself

CloudWatch

Alarm


### Routing Policies – Failover (Active-Passive)

EC2 Instance

(Primary)

Health Check

(mandatory)

DNS Requests

Failover

Client

Amazon

Route 53

EC2 Instance

(Secondary – Disaster Recovery)


### Routing Policies – Geolocation

A 11.22.33.44

- Different from Latency-based!
- This routing is based on user location
- Specify location by Continent, Country

or by US State (if there’s overlapping,

most precise location selected)

Default

A 99.11.22.33

- Should create a “Default” record (in

case there’s no match on location)

- Use cases: website localization, restrict

content distribution, load balancing, …

- Can be associated with Health Checks

A 55.66.77.88


### Routing Policies – Geoproximity

- Route traffic to your resources based on the geographic location of users and

resources

- Ability to shift more traffic to resources based on the defined bias
- To change the size of the geographic region, specify bias values:
- To expand (1 to 99) – more traffic to the resource
- To shrink (-1 to -99) – less traffic to the resource
- Resources can be:
- AWS resources (specify AWS region)
- Non-AWS resources (specify Latitude and Longitude)
- You must use Route 53 Traffic Flow to use this feature

### Routing Policies – Geoproximity

us-west-1 us-east-1

Bias: 0 Bias: 0


### Routing Policies – Geoproximity

us-west-1 us-east-1

Bias: 0 Bias: 50

Higher bias in us-east-1


### Routing Policies – IP-based Routing

User B User A

- Routing is based on clients’ IP addresses (200.5.4.100) (203.0.113.56)
- You provide a list of CIDRs for your clients

and the corresponding endpoints/locations

Route 53

(user-IP-to-endpoint mappings)

CIDR Collection

- Use cases: Optimize performance, reduce

Locations CIDR blocks

network costs… location-1 203.0.113.0/24

location-2 200.5.4.0/24

- Example: route end users from a particular

Records

ISP to a specific endpoint Record Name Value IP-based

example.com 1.2.3.4 location-1

example.com 5.6.7.8 location-2

EC2 Instance EC2 Instance

(5.6.7.8) (1.2.3.4)


| Locations | CIDR blocks |
| --- | --- |
| location-1 | 203.0.113.0/24 |
| location-2 | 200.5.4.0/24 |


| Record Name | Value | IP-based |
| --- | --- | --- |
| example.com | 1.2.3.4 | location-1 |
| example.com | 5.6.7.8 | location-2 |


### Routing Policies – Multi-Value

- Use when routing traffic to multiple resources
- Route 53 return multiple values/resources
- Can be associated with Health Checks (return only values for healthy resources)
- Up to 8 healthy records are returned for each Multi-Value query
- Multi-Value is not a substitute for having an ELB

### Domain Registar vs. DNS Ser vice

- You buy or register your domain name with a Domain Registrar typically by

paying annual charges (e.g., GoDaddy, Amazon Registrar Inc., …)

- The Domain Registrar usually provides you with a DNS service to manage

your DNS records

- But you can use another DNS service to manage your DNS records
- Example: purchase the domain from GoDaddy and use Route 53 to manage

your DNS records

purchase

example.com manage DNS records

User

Amazon

Route 53


### GoDaddy as Registrar & Route 53 as DNS Service

Amazon Public Hosted Zone

Route 53 stephanetheteacher.com


### 3rd Par ty Registrar with Amazon Route 53

- If you buy your domain on a 3rd party registrar, you can still use Route

53 as the DNS Service provider

1. Create a Hosted Zone in Route 53

2. Update NS Records on 3rd party website to use Route 53 Name

Servers

- Domain Registrar != DNS Service
- But every Domain Registrar usually comes with some DNS features

### Route 53 – Hybrid DNS

- By default, Route 53 Resolver

automatically answers DNS queries for:

Public Name Server

- Local domain names for EC2 instances
- Records in Private Hosted Zones
- Records in public Name Servers

Region

- Hybrid DNS – resolving DNS queries

between VPC (Route 53 Resolver) and

Private Hosted Zone

your networks (other DNS Resolvers)

- Networks can be:

Route 53

- VPC itself / Peered VPC

Resolver EC2 Instance

- On-premises Network (connected through (ec2-192-0-2-44.compute-1.amazonaws.com)

Direct Connect or AWS VPN)


### Route 53 – Resolver Endpoints

- Inbound Endpoint – allows your DNS Resolvers to resolve domain names for

AWS resources (e.g., EC2 instances) and records in Private Hosted Zones

us-east-1 On-Premises Data Center

Private Hosted Zone

(aws.private)

lookup

Query

app.aws.private?

Private Subnet

DNS Resolvers

Resolver (onpremise.private) Route 53

EC2 Instance Inbound Endpoint

Resolver

DNS Query

(app.aws.private)

app.aws.private?

VPN or DX connection

Server

(web.onpremise.private)


### Route 53 – Resolver Endpoints

- Outbound Endpoint
- Route 53 Resolver forwards DNS queries to your DNS Resolvers

us-east-1 On-Premises Data Center

Private Hosted Zone

(aws.private)

web.o D np N r S e Q m u is e e r . y p

Subnet

(a E p C p 2 .a I w n s s .p ta ri n va c t e e) web.on D p N r S e m

private? DNS Resolvers

(onpremise.private)

Resolver

Route 53

Outbound Endpoint

Resolver

VPN or DX connection

Server

(web.onpremise.private)


---

## Classic Solutions Architecture


### Section Introduction

- These solutions architectures are the best part of this course
- Let’s understand how all the technologies we’ve seen work together
- This is a section you need to be 100% comfortable with
- We’ll see the progression of a Solution’s architect mindset through many

sample case studies:

- WhatIsTheTime.Com
- MyClothes.Com
- MyWordPress.Com
- Instantiating applications quickly
- Beanstalk

### Stateless Web App: WhatIsTheTime.com

- WhatIsTheTime.com allows people to know what time it is
- We don’t need a database
- We want to start small and can accept downtime
- We want to fully scale vertically and horizontally, no downtime
- Let’s go through the Solutions Architect journey for this app
- Let’s see how we can proceed!

### Stateless web app: What time is it?

Star ting simple

Elastic IP Address

What time is it?

5:30 pm!

User

Public EC2


### Stateless web app: What time is it?

Scaling ver tically

What time is it?

Elastic IP Address

7:30 pm!

What time is it?

Downtime while upgrading to M5

5:30 pm!

User

What time is it? Public EC2

6:30 pm!


### Stateless web app: What time is it?

Scaling horizontally

What time is it?

7:30 pm!

What time is it?

5:30 pm!

User

What time is it?

6:30 pm!


### Stateless web app: What time is it?

Scaling horizontally

DNS Query What time is it?

Public EC2 instance,

For api.whatisthetime.com

No Elastic IP

A Record

TTL 1 hour 7:30 pm!

What time is it?

5:30 pm!

What time is it?

6:30 pm!


### Stateless web app: What time is it?

Scaling horizontally, adding and removing instances

DNS Query What time is it?

For api.whatisthetime.com

INSTANCE IS GONE!

A Record

TTL 1 hour 7:30 pm!

What time is it?

Public EC2 instance,

No Elastic IP

5:30 pm!

What time is it?

6:30 pm!


### Stateless web app: What time is it?

Scaling horizontally, with a load balancer

What time is it?

Availability zone 1 Availability zone 1

Restricted

Security groups rules

DNS Query

ELB +

For api.whatisthetime.com

Health Checks

Alias Record

Private

EC2 instances


### Stateless web app: What time is it?

Scaling horizontally, with an auto-scaling group

What time is it?

Availability zone 1 Availability zone 1

Auto Scaling group

DNS Query

ELB +

For api.whatisthetime.com

Health Checks

Alias Record

Private

EC2 instances


|  | Availability zone 1 |  |
| --- | --- | --- |
|  | Auto Scaling group |  |
|  | Private EC2 instances |  |


### Stateless web app: What time is it?

Making our app multi-AZ

Auto Scaling group

What time is it?

Availability zone 1 to 3 Availability zone 1

Availability zone 2

DNS Query

ELB +

For api.whatisthetime.com

Health Checks

Alias Record

+ Multi AZ

Availability zone 3


### Minimum 2 AZ => Let’s reser ve capacity

Auto Scaling group

Availability zone 1 to 3 Availability zone 1

Availability zone 2

DNS Query

ELB +

For api.whatisthetime.com

Health Checks

Alias Record

+ Multi AZ

Minimum capacity

= reserved instances

= cost savings


### In this lecture we’ve discussed…

- Public vs Private IP and EC2 instances
- Elastic IP vs Route 53 vs Load Balancers
- Route 53 TTL, A records and Alias Records
- Maintaining EC2 instances manually vs Auto Scaling Groups
- Multi AZ to survive disasters
- ELB Health Checks
- Security Group Rules
- Reservation of capacity for costing savings when possible

### Stateful Web App: MyClothes.com

- MyClothes.com allows people to buy clothes online.
- There’s a shopping cart
- Our website is having hundreds of users at the same time
- We need to scale, maintain horizontal scalability and keep our web

application as stateless as possible

- Users should not lose their shopping cart
- Users should have their details (address, etc) in a database
- Let’s see how we can proceed!

### Stateful Web App: MyClothes.com

Auto Scaling group

Availability zone 1

Multi AZ

Availability zone 2

Availability zone 3


### Stateful Web App: MyClothes.com

Introduce Stickiness (Session Affinity)

Auto Scaling group

Availability zone 1

Multi AZ

Availability zone 2

ELB Stickiness

Availability zone 3


### Stateful Web App: MyClothes.com

Introduce User Cookies

Auto Scaling group

Availability zone 1

Multi AZ

Send shopping cart Stateless

content in Web Cookies HTTP requests are heavier

Availability zone 2

Security risk

(cookies can be altered)

Cookies must be validated

Cookies must be less than 4KB

Availability zone 3


### Stateful Web App: MyClothes.com

Introduce Ser ver Session

ElastiCache

Auto Scaling group

Availability zone 1

Multi AZ

Send session_id in

Web Cookies

Store / retrieve

Availability zone 2

session data

Availability zone 3

Amazon DynamoDB

(alternative)


### Stateful Web App: MyClothes.com

Storing User Data in a database

ElastiCache

Auto Scaling group

Availability zone 1

Multi AZ

Availability zone 2

Store / retrieve user data

(address, name, etc)

Availability zone 3

Amazon RDS


### Stateful Web App: MyClothes.com

Scaling Reads

ElastiCache

Auto Scaling group

Availability zone 1

Multi AZ

Availability zone 2 RDS

Master

(writes)

replication

Availability zone 3

Read Replicas


### Stateful Web App: MyClothes.com

Scaling Reads (Alternative) – Lazy Loading

ElastiCache

Auto Scaling group

Availability zone 1 cache

Multi AZ

Read from cache

Availability zone 2

Read/write

Availability zone 3


### Stateful Web App: MyClothes.com

Multi AZ – Sur vive disasters

ElastiCache

Multi AZ

Auto Scaling group

Availability zone 1

Multi AZ

Availability zone 2

Availability zone 3

Multi AZ


### Stateful Web App: MyClothes.com

Security Groups

Restrict traffic to ElastiCache

Security group from the

EC2 security group

Auto Scaling group

Availability zone 1 ElastiCache

Multi AZ

Open HTTP / HTTPS

to 0.0.0.0/0

Availability zone 2

Availability zone 3

Restrict traffic to RDS

Restrict traffic to EC2

Security group from the

Security group from the LB

EC2 security group


### In this lecture we’ve discussed…

3-tier architectures for web applications

- ELB sticky sessions
- Web clients for storing cookies and making our web app stateless
- ElastiCache
- For storing sessions (alternative: DynamoDB)
- For caching data from RDS
- Multi AZ
- RDS
- For storing user data
- Read replicas for scaling reads
- Multi AZ for disaster recovery
- Tight Security with security groups referencing each other

### Stateful Web App: MyWordPress.com

- We are trying to create a fully scalable WordPress website
- We want that website to access and correctly display picture uploads
- Our user data, and the blog content should be stored in a MySQL database.
- Let’s see how we can achieve this!

### Stateful Web App: MyWordPress.com

RDS layer

Auto Scaling group

Availability zone 1

Multi AZ

Availability zone 2

Multi AZ

Availability zone 3


### Stateful Web App: MyWordPress.com

Scaling with Aurora: Multi AZ & Read Replicas

Auto Scaling group

Availability zone 1

Multi AZ

Availability zone 2

Aurora MySQL

Multi AZ

Availability zone 3

Read Replicas


### Stateful Web App: MyWordPress.com

Storing images with EBS

Multi AZ

Availability zone 1

Send image

Amazon EBS

Volume


### Stateful Web App: MyWordPress.com

Storing images with EBS

Availability zone 1

Multi AZ

Amazon EBS

Volume

Send image

Availability zone 2

Amazon EBS

Volume


### Stateful Web App: MyWordPress.com

Storing images with EFS

Availability zone 1

Multi AZ

Send image

Availability zone 2


### In this lecture we’ve discussed…

- Aurora Database to have easy Multi-AZ and Read-Replicas
- Storing data in EBS (single instance application)
- Vs Storing data in EFS (distributed application)

### Instantiating Applications quickly

- When launching a full stack (EC2, EBS, RDS), it can take time to:
- Install applications
- Insert initial (or recovery) data
- Configure everything
- Launch the application
- We can take advantage of the cloud to speed that up!

### Instantiating Applications quickly

- EC2 Instances:
- Use a Golden AMI: Install your applications, OS dependencies etc.. beforehand

and launch your EC2 instance from the Golden AMI

- Bootstrap using User Data: For dynamic configuration, use User Data scripts
- Hybrid: mix Golden AMI and User Data (Elastic Beanstalk)
- RDS Databases:
- Restore from a snapshot: the database will have schemas and data ready!
- EBS Volumes:
- Restore from a snapshot: the disk will already be formatted and have data!

### Typical architecture: Web App 3-tier

Route 53

ElastiCache

Auto Scaling group

Availability zone 1

Multi AZ

Store / retrieve

session data

Availability zone 2

+ Cached data

Availability zone 3

Amazon RDS

Read / write data

PUBLIC SUBNET PRIVATE SUBNET DATA SUBNET


### Developer problems on AWS

- Managing infrastructure
- Deploying Code
- Configuring all the databases, load balancers, etc
- Scaling concerns
- Most web apps have the same architecture (ALB + ASG)
- All the developers want is for their code to run!
- Possibly, consistently across different applications and environments

### Elastic Beanstalk – Over view

- Elastic Beanstalk is a developer centric view of deploying an application

on AWS

- It uses all the component’s we’ve seen before: EC2, ASG, ELB, RDS, …
- Managed service
- Automatically handles capacity provisioning, load balancing, scaling, application

health monitoring, instance configuration, …

- Just the application code is the responsibility of the developer
- We still have full control over the configuration
- Beanstalk is free but you pay for the underlying instances

### Elastic Beanstalk – Components

- Application: collection of Elastic Beanstalk components (environments,

versions, configurations, …)

- Application Version: an iteration of your application code
- Environment
- Collection of AWS resources running an application version (only one application

version at a time)

- Tiers: Web Server Environment Tier & Worker Environment Tier
- You can create multiple environments (dev, test, prod, …)

update version

Create Upload Launch Manage

Application Version Environment Environment

deploy new version


### Elastic Beanstalk – Suppor ted Platforms

- Go • Ruby
- Java SE • Packer Builder
- Java with Tomcat • Single Container Docker
- .NET Core on Linux • Multi-container Docker
- .NET on Windows Server • Preconfigured Docker
- Node.js
- PHP
- Python

### Web Ser ver Tier vs. Worker Tier

Web Environment

Worker Environment

(myapp.us-east-1.elasticbeanstalk.com)

SQS Queue

Availability Zone 1 Availability Zone 2 Availability Zone 1 Availability Zone 2

SQS message SQS message

pull

messages

Security Group Security Group

Auto Scaling group

Auto Scaling group

EC2 Instance EC2 Instance

EC2 Instance EC2 Instance

(Worker) (Worker)

(Web Server) (Web Server)

- Scale based on the number of SQS messages
- Can push messages to SQS queue from

another Web Server Tier


|  |  |  |  |
| --- | --- | --- | --- |
| EC2 Instance (Worker) |  | Auto Scaling group | EC2 Instance (Worker) |


| Security Group EC2 Instance (Web Server) | Auto Scaling group | Security Group EC2 Instance (Web Server) |
| --- | --- | --- |


### Elastic Beanstalk Deployment Modes

High Availability with Load Balancer

Single Instance

Great for prod

Great for dev

Availability Zone 1 Availability Zone 1 ALB Availability Zone 2

Elastic IP

Auto Scaling Group

EC2 Instance EC2 Instance EC2 Instance

RDS Master RDS Master RDS Standby


| Availability Zone 1 EC2 Instance RDS Master |  | Zone 1 |  | Availabi |
| --- | --- | --- | --- | --- |
|  | EC2 Instance |  | Auto Scaling Group | EC2 Instance |


---

## Amazon S3


### Section introduction

- Amazon S3 is one of the main building blocks of AWS
- It’s advertised as ”infinitely scaling” storage
- Many websites use Amazon S3 as a backbone
- Many AWS services use Amazon S3 as an integration as well
- We’ll have a step-by-step approach to S3

### Amazon S3 Use cases

- Backup and storage
- Disaster Recovery
- Archive

Nasdaq stores 7 years of

data into S3 Glacier

- Hybrid Cloud storage
- Application hosting
- Media hosting
- Data lakes & big data analytics

Sysco runs analytics on

- Software delivery

its data and gain business

insights

- Static website

### Amazon S3 - Buckets

- Amazon S3 allows people to store objects (files) in “buckets” (directories)
- Buckets must have a globally unique name (across all regions all accounts)
- Buckets are defined at the region level
- S3 looks like a global service but buckets are created in a region
- Naming convention
- No uppercase, No underscore
- 3-63 characters long
- Not an IP
- Must start with lowercase letter or number
- Must NOT start with the prefix xn--

S3 Bucket

- Must NOT end with the suffix -s3alias

### Amazon S3 - Objects

- Objects (files) have a Key
- The key is the FULL path:
- s3://my-bucket/my_file.txt
- s3://my-bucket/my_folder1/another_folder/my_file.txt

Object

- The key is composed of prefix + object name
- s3://my-bucket/my_folder1/another_folder/my_file.txt
- There’s no concept of “directories” within buckets

(although the UI will trick you to think otherwise)

S3 Bucket

- Just keys with very long names that contain slashes (“/”)

with Objects


### Amazon S3 – Objects (cont.)

- Object values are the content of the body:
- Max. Object Size is 5TB (5000GB)
- If uploading more than 5GB, must use “multi-part upload”
- Metadata (list of text key / value pairs – system or user metadata)
- Tags (Unicode key / value pair – up to 10) – useful for security / lifecycle
- Version ID (if versioning is enabled)

### Amazon S3 – Security

- User-Based
- IAM Policies – which API calls should be allowed for a specific user from IAM
- Resource-Based
- Bucket Policies – bucket wide rules from the S3 console - allows cross account
- Object Access Control List (ACL) – finer grain (can be disabled)
- Bucket Access Control List (ACL) – less common (can be disabled)
- Note: an IAM principal can access an S3 object if
- The user IAM permissions ALLOW it OR the resource policy ALLOWS it
- AND there’s no explicit DENY
- Encryption: encrypt objects in Amazon S3 using encryption keys

### S3 Bucket Policies

- JSON based policies
- Resources: buckets and objects
- Effect: Allow / Deny
- Actions: Set of API to Allow or Deny
- Principal: The account or user to apply the

policy to

- Use S3 bucket for policy to:
- Grant public access to the bucket
- Force objects to be encrypted at upload
- Grant access to another account (Cross

Account)


### Example: Public Access - Use Bucket Policy

S3 Bucket Policy

Allows Public Access

Anonymous www website visitor S3 Bucket


### Example: User Access to S3 – IAM permissions

IAM Policy

IAM User

S3 Bucket


### Example: EC2 instance access - Use IAM Roles

IAM permissions

EC2 Instance Role

EC2 Instance

S3 Bucket


### Advanced: Cross-Account Access –

Use Bucket Policy

S3 Bucket Policy

Allows Cross-Account

IAM User

Other AWS account

S3 Bucket


### Bucket settings for Block Public Access

- These settings were created to prevent company data leaks
- If you know your bucket should never be public, leave these on
- Can be set at the account level

### Amazon S3 – Static Website Hosting

User

- S3 can host static websites and have them accessible on

the Internet

http://demo-bucket.s3-website-us-west-2.amazonaws.com

http://demo-bucket.s3-website.us-west-2.amazonaws.com

- The website URL will be (depending on the region)
- http://bucket-name.s3-website-aws-region.amazonaws.com

OR us-west-2

- http://bucket-name.s3-website.aws-region.amazonaws.com

S3 Bucket

- If you get a 403 Forbidden error, make sure the bucket

(demo-bucket)

policy allows public reads!


### Amazon S3 - Versioning

User

- You can version your files in Amazon S3
- It is enabled at the bucket level

upload

- Same key overwrite will change the “version”: 1, 2, 3….
- It is best practice to version your buckets
- Protect against unintended deletes (ability to restore a version) S3 Bucket

(my-bucket)

- Easy roll back to previous version

Version 1 Version 2

- Notes:

Version 3

- Any file that is not versioned prior to enabling versioning will

have version “null”

s3://my-bucket/my-file.docx

- Suspending versioning does not delete the previous versions

### Amazon S3 – Replication (CRR & SRR)

- Must enable Versioning in source and destination buckets
- Cross-Region Replication (CRR)

S3 Bucket

- Same-Region Replication (SRR)

(eu-west-1)

- Buckets can be in different AWS accounts
- Copying is asynchronous

asynchronous

- Must give proper IAM permissions to S3

replication

- Use cases:

S3 Bucket

- CRR – compliance, lower latency access, replication across accounts

(us-east-2)

- SRR – log aggregation, live replication between production and test

accounts


### Amazon S3 – Replication (Notes)

- After you enable Replication, only new objects are replicated
- Optionally, you can replicate existing objects using S3 Batch Replication
- Replicates existing objects and objects that failed replication
- For DELETE operations
- Can replicate delete markers from source to target (optional setting)
- Deletions with a version ID are not replicated (to avoid malicious deletes)
- There is no “chaining” of replication
- If bucket 1 has replication into bucket 2, which has replication into bucket 3
- Then objects created in bucket 1 are not replicated to bucket 3

### S3 Storage Classes

- Amazon S3 Standard - General Purpose
- Amazon S3 Standard-Infrequent Access (IA)
- Amazon S3 One Zone-Infrequent Access
- Amazon S3 Glacier Instant Retrieval
- Amazon S3 Glacier Flexible Retrieval
- Amazon S3 Glacier Deep Archive
- Amazon S3 Intelligent Tiering
- Can move between classes manually or using S3 Lifecycle configurations

### S3 Durability and Availability

- Durability:
- High durability (99.999999999%, 11 9’s) of objects across multiple AZ
- If you store 10,000,000 objects with Amazon S3, you can on average expect to

incur a loss of a single object once every 10,000 years

- Same for all storage classes
- Availability:
- Measures how readily available a service is
- Varies depending on storage class
- Example: S3 standard has 99.99% availability = not available 53 minutes a year

### S3 Standard – General Purpose

- 99.99% Availability
- Used for frequently accessed data
- Low latency and high throughput
- Sustain 2 concurrent facility failures
- Use Cases: Big Data analytics, mobile & gaming applications, content

distribution…


### S3 Storage Classes – Infrequent Access

- For data that is less frequently accessed, but requires rapid access when needed
- Lower cost than S3 Standard
- Amazon S3 Standard-Infrequent Access (S3 Standard-IA)
- 99.9% Availability
- Use cases: Disaster Recovery, backups
- Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA)
- High durability (99.999999999%) in a single AZ; data lost when AZ is destroyed
- 99.5% Availability
- Use Cases: Storing secondary backup copies of on-premises data, or data you can recreate

### Amazon S3 Glacier Storage Classes

- Low-cost object storage meant for archiving / backup
- Pricing: price for storage + object retrieval cost
- Amazon S3 Glacier Instant Retrieval
- Millisecond retrieval, great for data accessed once a quarter
- Minimum storage duration of 90 days
- Amazon S3 Glacier Flexible Retrieval (formerly Amazon S3 Glacier):
- Expedited (1 to 5 minutes), Standard (3 to 5 hours), Bulk (5 to 12 hours) – free
- Minimum storage duration of 90 days
- Amazon S3 Glacier Deep Archive – for long term storage:
- Standard (12 hours), Bulk (48 hours)
- Minimum storage duration of 180 days

### S3 Intelligent-Tiering

- Small monthly monitoring and auto-tiering fee
- Moves objects automatically between Access Tiers based on usage
- There are no retrieval charges in S3 Intelligent-Tiering
- Frequent Access tier (automatic): default tier
- Infrequent Access tier (automatic): objects not accessed for 30 days
- Archive Instant Access tier (automatic): objects not accessed for 90 days
- Archive Access tier (optional): configurable from 90 days to 700+ days
- Deep Archive Access tier (optional): config. from 180 days to 700+ days

### S3 Storage Classes Comparison

Intelligent- Glacier Instant Glacier Flexible Glacier Deep

Standard Standard-IA One Zone-IA

Tiering Retrieval Retrieval Archive

Durability 99.999999999% == (11 9’s)

Availability 99.99% 99.9% 99.9% 99.5% 99.9% 99.99% 99.99%

Availability SLA 99.9% 99% 99% 99% 99% 99.9% 99.9%

Availability

>= 3 >= 3 >= 3 1 >= 3 >= 3 >= 3

Zones

Min. Storage

None None 30 Days 30 Days 90 Days 90 Days 180 Days

Duration Charge

Min. Billable

None None 128 KB 128 KB 128 KB 40 KB 40 KB

Object Size

Retrieval Fee None None Per GB retrieved Per GB retrieved Per GB retrieved Per GB retrieved Per GB retrieved

https://aws.amazon.com/s3/storage-classes/


|  | Standard | Intelligent- Tiering | Standard-IA | One Zone-IA | Glacier Instant Retrieval | Glacier Flexible Retrieval | Glacier Deep Archive |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Durability | 99.999999999% == (11 9’s) |  |  |  |  |  |  |
| Availability | 99.99% | 99.9% | 99.9% | 99.5% | 99.9% | 99.99% | 99.99% |
| Availability SLA | 99.9% | 99% | 99% | 99% | 99% | 99.9% | 99.9% |
| Availability Zones | >= 3 | >= 3 | >= 3 | 1 | >= 3 | >= 3 | >= 3 |
| Min. Storage Duration Charge | None | None | 30 Days | 30 Days | 90 Days | 90 Days | 180 Days |
| Min. Billable Object Size | None | None | 128 KB | 128 KB | 128 KB | 40 KB | 40 KB |
| Retrieval Fee | None | None | Per GB retrieved | Per GB retrieved | Per GB retrieved | Per GB retrieved | Per GB retrieved |


### S3 Storage Classes – Price Comparison

Example: us-east-1

Glacier Instant Glacier Flexible Glacier Deep

Standard Intelligent-Tiering Standard-IA One Zone-IA

Retrieval Retrieval Archive

Storage Cost

$0.023 $0.0025 - $0.023 $0.0125 $0.01 $0.004 $0.0036 $0.00099

(per GB per month)

GET: $0.0004

GET: $0.0004

POST: $0.03

POST: $0.05

Retrieval Cost GET: $0.0004 GET: $0.0004 GET: $0.001 GET: $0.001 GET: $0.01

(per 1000 request) POST: $0.005 POST: $0.005 POST: $0.01 POST: $0.01 POST: $0.02 Expedited: $10

Standard: $0.10

Standard: $0.05

Bulk: $0.025

Bulk: free

Expedited (1 – 5 mins)

Standard (12 hours)

Retrieval Time Instantaneous Standard (3 – 5 hours)

Bulk (48 hours)

Bulk (5 – 12 hours)

Monitoring Cost

$0.0025

(per 1000 objects)

https://aws.amazon.com/s3/pricing/


|  | Standard | Intelligent-Tiering |  | Standard-IA | One Zone-IA | Glacier Instant Retrieval |  | Glacier Flexible Retrieval | Glacier Deep Archive |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Storage Cost (per GB per month) | $0.023 | $0.0025 - $0.023 |  | $0.0125 | $0.01 | $0.004 |  | $0.0036 | $0.00099 |
| Retrieval Cost (per 1000 request) | GET: $0.0004 POST: $0.005 | GET: $0.0004 POST: $0.005 |  | GET: $0.001 POST: $0.01 | GET: $0.001 POST: $0.01 | GET: $0.01 POST: $0.02 |  | GET: $0.0004 POST: $0.03 Expedited: $10 Standard: $0.05 Bulk: free | GET: $0.0004 POST: $0.05 Standard: $0.10 Bulk: $0.025 |
| Retrieval Time | Instantaneous |  |  |  |  |  |  | Expedited (1 – 5 mins) Standard (3 – 5 hours) Bulk (5 – 12 hours) | Standard (12 hours) Bulk (48 hours) |
| Monitoring Cost (per 1000 objects) |  | $0.0025 |  |  |  |  |  |  |  |


### S3 Express One Zone

- High performance, single Availability Zone storage class
- Objects stored in a Directory Bucket (bucket in a single AZ)
- Handle 100,000s requests per second with single-digit millisecond

Region (us-east-1)

latency

- Up to 10x better performance than S3 Standard (50% lower

Availability Zone (AZ 4)

costs)

- High Durability (99.999999999%) and Availability (99.95%)
- Co-locate your storage and compute resources in the same AZ

(reduces latency)

- Use cases: latency-sensitive apps, data-intensive apps, AI & ML stephane--use1-az4--x-s3

training, financial modeling, media processing, HPC…

- Best integrated with SageMaker Model Training, Athena, EMR,

Glue…


---

## Amazon S3 – Advanced


### Amazon S3 – Moving between Storage Classes

- You can transition objects between

storage classes

Standard

Standard IA

- For infrequently accessed object,

move them to Standard IA

Intelligent Tiering

- For archive objects that you don’t

One-Zone IA

need fast access to, move them to

Glacier or Glacier Deep Archive

Glacier Instant Retrieval

Glacier Flexible Retrieval

- Moving objects can be automated

using a Lifecycle Rules

Glacier Deep Archive


|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |


### Amazon S3 – Lifecycle Rules

- Transition Actions – configure objects to transition to another storage class
- Move objects to Standard IA class 60 days after creation
- Move to Glacier for archiving after 6 months
- Expiration actions – configure objects to expire (delete) after some time
- Access log files can be set to delete after a 365 days
- Can be used to delete old versions of files (if versioning is enabled)
- Can be used to delete incomplete Multi-Part uploads
- Rules can be created for a certain prefix (example: s3://mybucket/mp3/*)
- Rules can be created for certain objects Tags (example: Department: Finance)

### Amazon S3 – Lifecycle Rules (Scenario 1)

- Your application on EC2 creates images thumbnails after profile photos

are uploaded to Amazon S3. These thumbnails can be easily recreated,

and only need to be kept for 60 days. The source images should be able

to be immediately retrieved for these 60 days, and afterwards, the user

can wait up to 6 hours. How would you design this?

- S3 source images can be on Standard, with a lifecycle configuration to

transition them to Glacier after 60 days

- S3 thumbnails can be on One-Zone IA, with a lifecycle configuration to

expire them (delete them) after 60 days


### Amazon S3 – Lifecycle Rules (Scenario 2)

- A rule in your company states that you should be able to recover your

deleted S3 objects immediately for 30 days, although this may happen

rarely. After this time, and for up to 365 days, deleted objects should be

recoverable within 48 hours.

- Enable S3 Versioning in order to have object versions, so that “deleted

objects” are in fact hidden by a “delete marker” and can be recovered

- Transition the “noncurrent versions” of the object to Standard IA
- Transition afterwards the “noncurrent versions” to Glacier Deep Archive

### Amazon S3 Analytics – Storage Class Analysis

- Help you decide when to transition objects to

S3 Bucket

the right storage class

- Recommendations for Standard and Standard IA
- Does NOT work for One-Zone IA or Glacier
- Report is updated daily

S3 Analytics

- 24 to 48 hours to start seeing data analysis

.csv report

- Good first step to put together Lifecycle Rules

Date StorageClass ObjectAge

(or improve them)!

8/22/2022 STANDARD 000-014

8/25/2022 STANDARD 030-044

9/6/2022 STANDARD 120-149


| Date | StorageClass | ObjectAge |
| --- | --- | --- |
| 8/22/2022 | STANDARD | 000-014 |
| 8/25/2022 | STANDARD | 030-044 |
| 9/6/2022 | STANDARD | 120-149 |


### S3 – Requester Pays

Standard Bucket

- In general, bucket owners pay for all

Owner Owner Requester

Amazon S3 storage and data transfer $$ Storage Cost $$ Networking Cost

costs associated with their bucket

download

- With Requester Pays buckets, the

requester instead of the bucket owner

pays the cost of the request and the

Requester Pays Bucket

data download from the bucket

Owner Requester

- Helpful when you want to share large

$$ Storage Cost $$ Networking Cost

datasets with other accounts

download

- The requester must be authenticated

in AWS (cannot be anonymous)


### S3 Event Notifications

- S3:ObjectCreated, S3:ObjectRemoved,

S3:ObjectRestore, S3:Replication…

- Object name filtering possible (*.jpg) SNS
- Use case: generate thumbnails of images

uploaded to S3

events

- Can create as many “S3 events” as desired

Amazon S3

- S3 event notifications typically deliver events

in seconds but can sometimes take a minute

or longer

Lambda Function


### S3 Event Notifications – IAM Permissions

SNS Resource (Access) Policy

events

Amazon S3

SQS Resource (Access) Policy

Lambda Function

Lambda Resource Policy


### S3 Event Notifications

with Amazon EventBridge

events All events rules

Over 18

AWS services

as destinations

Amazon S3 Amazon

bucket EventBridge

- Advanced filtering options with JSON rules (metadata, object size, name...)
- Multiple Destinations – ex Step Functions, Kinesis Streams / Firehose…
- EventBridge Capabilities – Archive, Replay Events, Reliable delivery

### S3 – Baseline Performance

- Amazon S3 automatically scales to high request rates, latency 100-200 ms
- Your application can achieve at least 3,500 PUT/COPY/POST/DELETE or

5,500 GET/HEAD requests per second per prefix in a bucket.

- There are no limits to the number of prefixes in a bucket.
- Example (object path => prefix):
- bucket/folder1/sub1/file => /folder1/sub1/
- bucket/folder1/sub2/file => /folder1/sub2/
- bucket/1/file => /1/
- bucket/2/file => /2/
- If you spread reads across all four prefixes evenly, you can achieve 22,000

requests per second for GET and HEAD


### S3 Performance

- Multi-Part upload: • S3 Transfer Acceleration
- recommended for files > 100MB, • Increase transfer speed by transferring

must use for files > 5GB file to an AWS edge location which will

forward the data to the S3 bucket in the

- Can help parallelize uploads (speed

target region

up transfers)

- Compatible with multi-part upload

Divide

Parallel uploads

In parts

Fast Fast

(public www) (private AWS)

File in USA Edge Location S3 Bucket

Amazon S3 USA

Australia

BIG file


### S3 Performance – S3 Byte-Range Fetches

- Parallelize GETs by requesting specific

byte ranges

- Better resilience in case of failures

Can be used to retrieve only partial

Can be used to speed up downloads data (for example the head of a file)

File in S3 File in S3

Byte-range request for header

(first XX bytes)

Part 1 Part 2 … Part N header

Requests in parallel


### S3 Batch Operations

S3 Inventory

- Perform bulk operations on existing S3 objects with a

single request, example:

- Modify object metadata & properties

Objects List Report

- Copy objects between S3 buckets
- Encrypt un-encrypted objects
- Modify ACLs, tags

Athena

- Restore objects from S3 Glacier

filter

- Invoke Lambda function to perform custom action on

each object

filtered list

- A job consists of a list of objects, the action to

operation

perform, and optional parameters

parameters S3 Batch

- S3 Batch Operations manages retries, tracks progress,

sends completion notifications, generate reports … Operations

User

- You can use S3 Inventory to get object list and use

Athena to query and filter your objects

Processed Objects


### S3 – Storage Lens

- Understand, analyze, and optimize storage across entire AWS Organization
- Discover anomalies, identify cost efficiencies, and apply data protection best

practices across entire AWS Organization (30 days usage & activity metrics)

- Aggregate data for Organization, specific accounts, regions, buckets, or prefixes
- Default dashboard or create your own dashboards
- Can be configured to export metrics daily to an S3 bucket (CSV, Parquet)

Organization

Summary Insights

Accounts

Data Protection

S3 Storage Lens Regions

Cost Efficiency

Buckets

Configure Aggregate Analyze Optimize

(Dashboard)


### Storage Lens – Default Dashboard

- Visualize summarized insights and trends for both free and advanced metrics
- Default dashboard shows Multi-Region and Multi-Account data
- Preconfigured by Amazon S3
- Can’t be deleted, but can be disabled

https://aws.amazon.com/blogs/aws/s3-storage-lens/

https://aws.amazon.com/blogs/aws/s3-storage-lens/


### Storage Lens – Metrics

- Summary Metrics
- General insights about your S3 storage
- StorageBytes, ObjectCount…
- Use cases: identify the fastest-growing (or not used) buckets and prefixes
- Cost-Optimization Metrics
- Provide insights to manage and optimize your storage costs
- NonCurrentVersionStorageBytes, IncompleteMultipartUploadStorageBytes…
- Use cases: identify buckets with incomplete multipart uploaded older than 7

days, Identify which objects could be transitioned to lower-cost storage class


### Storage Lens – Metrics

- Data-Protection Metrics
- Provide insights for data protection features
- VersioningEnabledBucketCount, MFADeleteEnabledBucketCount, SSEKMSEnabledBucketCount,

CrossRegionReplicationRuleCount…

- Use cases: identify buckets that aren’t following data-protection best practices
- Access-management Metrics
- Provide insights for S3 Object Ownership
- ObjectOwnershipBucketOwnerEnforcedBucketCount…
- Use cases: identify which Object Ownership settings your buckets use
- Event Metrics
- Provide insights for S3 Event Notifications
- EventNotificationEnabledBucketCount (identify which buckets have S3 Event Notifications

configured)


### Storage Lens – Metrics

- Performance Metrics
- Provide insights for S3 Transfer Acceleration
- TransferAccelerationEnabledBucketCount (identify which buckets have S3 Transfer

Acceleration enabled)

- Activity Metrics
- Provide insights about how your storage is requested
- AllRequests, GetRequests, PutRequests, ListRequests, BytesDownloaded…
- Detailed Status Code Metrics
- Provide insights for HTTP status codes
- 200OKStatusCount, 403ForbiddenErrorCount, 404NotFoundErrorCount…

### Storage Lens – Free vs. Paid

- Free Metrics
- Automatically available for all customers
- Contains around 28 usage metrics
- Data is available for queries for 14 days
- Advanced Metrics and Recommendations
- Additional paid metrics and features
- Advanced Metrics – Activity, Advanced Cost

Optimization, Advanced Data Protection, Status

Code

- CloudWatch Publishing – Access metrics in

CloudWatch without additional charges

- Prefix Aggregation – Collect metrics at the prefix

level

- Data is available for queries for 15 months

---

## Amazon S3 – Security


### Amazon S3 – Object Encr yption

- You can encrypt objects in S3 buckets using one of 4 methods
- Server-Side Encryption (SSE)
- Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3) – Enabled by Default
- Encrypts S3 objects using keys handled, managed, and owned by AWS
- Server-Side Encryption with KMS Keys stored in AWS KMS (SSE-KMS)
- Leverage AWS Key Management Service (AWS KMS) to manage encryption keys
- Server-Side Encryption with Customer-Provided Keys (SSE-C)
- When you want to manage your own encryption keys
- Client-Side Encryption
- It’s important to understand which ones are for which situation for the exam

### Amazon S3 Encr yption – SSE-S3

- Encryption using keys handled, managed, and owned by AWS
- Object is encrypted server-side
- Encryption type is AES-256
- Must set header "x-amz-server-side-encryption": "AES256"
- Enabled by default for new buckets & new objects

Amazon S3

Object

upload

Encryption

HTTP(S) + Header

User

S3 Bucket

S3 Owned Key


### Amazon S3 Encr yption – SSE-KMS

- Encryption using keys handled and managed by AWS KMS (Key Management Service)
- KMS advantages: user control + audit key usage using CloudTrail
- Object is encrypted server side
- Must set header "x-amz-server-side-encryption": "aws:kms"

Amazon S3

Object

upload

Encryption

HTTP(S) + Header

User

S3 Bucket

KMS Key

AWS KMS


### SSE-KMS Limitation

- If you use SSE-KMS, you may be impacted

S3 Bucket KMS Key

by the KMS limits

API call

- When you upload, it calls the

GenerateDataKey KMS API

Upload / download

- When you download, it calls the Decrypt

SSE-KMS

KMS API

- Count towards the KMS quota per second

(5500, 10000, 30000 req/s based on region)

Users

- You can request a quota increase using the

Service Quotas Console


### Amazon S3 Encr yption – SSE-C

- Server-Side Encryption using keys fully managed by the customer outside of AWS
- Amazon S3 does NOT store the encryption key you provide
- HTTPS must be used
- Encryption key must provided in HTTP headers, for every HTTP request made

Amazon S3

Object

upload

Encryption

HTTPS ONLY

User

+ Key in Header

S3 Bucket

Client-Provided Key


### Amazon S3 Encr yption – Client-Side Encr yption

- Use client libraries such as Amazon S3 Client-Side Encryption Library
- Clients must encrypt data themselves before sending to Amazon S3
- Clients must decrypt data themselves when retrieving from Amazon S3
- Customer fully manages the keys and encryption cycle

Amazon S3

File

+ upload

Encryption

HTTP(S)

File

S3 Bucket

(encrypted)

Client Key


### Amazon S3 – Encr yption in transit (SSL/TLS)

- Encryption in flight is also called SSL/TLS
- Amazon S3 exposes two endpoints:
- HTTP Endpoint – non encrypted
- HTTPS Endpoint – encryption in flight
- HTTPS is recommended
- HTTPS is mandatory for SSE-C
- Most clients would use the HTTPS endpoint by default

### Amazon S3 – Force Encr yption in Transit

aws:SecureTranspor t

Account B

User

http

S3 Bucket

(my-bucket)

https

Bucket Policy

User


### Amazon S3 – Default Encryption vs. Bucket Policies

- SSE-S3 encryption is automatically applied to new objects stored in S3 bucket
- Optionally, you can “force encryption” using a bucket policy and refuse any API call

to PUT an S3 object without encryption headers (SSE-KMS or SSE-C)

- Note: Bucket Policies are evaluated before “Default Encryption”

### What is CORS?

- Cross-Origin Resource Sharing (CORS)
- Origin = scheme (protocol) + host (domain) + port
- example: https://www.example.com (implied port is 443 for HTTPS, 80 for HTTP)
- Web Browser based mechanism to allow requests to other origins while

visiting the main origin

- Same origin: http://example.com/app1 & http://example.com/app2
- Different origins: http://www.example.com & http://other.example.com
- The requests won’t be fulfilled unless the other origin allows for the

requests, using CORS Headers (example: Access-Control-Allow-Origin)


### What is CORS?

OPTIONS /

Host: www.other.com

Origin: https://www.example.com

Preflight Request

Access-Control-Allow-Origin: https://www.example.com

Access-Control-Allow-Methods: GET, PUT, DELETE

HTTPS Request

Preflight Response

Web Browser

Web Server Web Server

(Origin) GET / (Cross-Origin)

https://www.example.com Host: www.other.com https://www.other.com

Origin: https://www.example.com

CORS Headers received already by the Origin

The Web Browser can make requests


### Amazon S3 – CORS

- If a client makes a cross-origin request on our S3 bucket, we need to enable

the correct CORS headers

- It’s a popular exam question
- You can allow for a specific origin or for * (all origins)

GET /index.html

Host: http://my-bucket-html.s3-website.us-west-2.amazonaws.com S3 Bucket

(my-bucket-html)

(Static Website Enabled)

index.html

GET /images/coffee.jpg

Web Browser Host: http://my-bucket-assets.s3-website.us-west-2.amazonaws.com

Origin: http://my-bucket-html.s3-website.us-west-2.amazonaws.com S3 Bucket

(my-bucket-assets)

(Static Website Enabled)

Access-Control-Allow-Origin: http://my-bucket-html.s3-website.us-west-2.amazonaws.com


### Amazon S3 – MFA Delete

- MFA (Multi-Factor Authentication) – force users to generate a code on a

device (usually a mobile phone or hardware) before doing important

operations on S3

- MFA will be required to:
- Permanently delete an object version

Google Authenticator

- Suspend Versioning on the bucket
- MFA won’t be required to:
- Enable Versioning
- List deleted versions MFA Hardware Device
- To use MFA Delete, Versioning must be enabled on the bucket
- Only the bucket owner (root account) can enable/disable MFA Delete

### S3 Access Logs

- For audit purpose, you may want to log all access to S3 buckets

requests

- Any request made to S3, from any account, authorized or denied,

will be logged into another S3 bucket

- That data can be analyzed using data analysis tools…
- The target logging bucket must be in the same AWS region

My-bucket

Log all

requests

- The log format is at:

https://docs.aws.amazon.com/AmazonS3/latest/dev/LogFormat.html

Logging Bucket


### S3 Access Logs: Warning

- Do not set your logging bucket to be the monitored bucket
- It will create a logging loop, and your bucket will grow exponentially

Logging loop

PutObject

App Bucket &

Logging Bucket

Do not try this at home J


### Amazon S3 – Pre-Signed URLs

Owner

- Generate pre-signed URLs using the S3 Console, AWS CLI or SDK
- URL Expiration
- S3 Console – 1 min up to 720 mins (12 hours)
- AWS CLI – configure expiration with --expires-in parameter in seconds

(default 3600 secs, max. 604800 secs ~ 168 hours)

- Users given a pre-signed URL inherit the permissions of the user

that generated the URL for GET / PUT

S3 Bucket

- Examples:

(Private)

- Allow only logged-in users to download a premium video from your S3

bucket

- Allow an ever-changing list of users to download files by generating URLs

dynamically

- Allow temporarily a user to upload a file to a precise location in your S3

bucket

User

pre-signed

generate


### S3 Glacier Vault Lock

- Adopt a WORM (Write Once Read

Object

Many) model

- Create a Vault Lock Policy
- Lock the policy for future edits

(can no longer be changed or deleted)

- Helpful for compliance and data

Vault Lock Policy

retention Object can’t be deleted


### S3 Object Lock (versioning must be enabled)

- Adopt a WORM (Write Once Read Many) model
- Block an object version deletion for a specified amount of time
- Retention mode - Compliance:
- Object versions can't be overwritten or deleted by any user, including the root user
- Objects retention modes can't be changed, and retention periods can't be shortened
- Retention mode - Governance:
- Most users can't overwrite or delete an object version or alter its lock settings
- Some users have special permissions to change the retention or delete the object
- Retention Period: protect the object for a fixed period, it can be extended
- Legal Hold:
- protect the object indefinitely, independent from retention period
- can be freely placed and removed using the s3:PutObjectLegalHold IAM permission

### S3 – Access Points

Policy

Grant R/W to

Users /finance prefix Finance

S3 Bucket

Access Point

(Finance) Simple Bucket

Policy

Policy

Grant R/W to

/finance/…

Users /sales prefix Sales

(Sales) Access Point

Policy /sales/…

Grant R to

Users entire bucket Analytics

(Analytics) Access Point

- Access Points simplify security management for S3 Buckets
- Each Access Point has:
- its own DNS name (Internet Origin or VPC Origin)
- an access point policy (similar to bucket policy) – manage security at scale

### S3 – Access Points – VPC Origin

- We can define the access

Access Point

point to be accessible S3 Bucket

EC2 Instance VPC Endpoint VPC Origin

only from within the VPC

- You must create a VPC

Endpoint to access the Endpoint Access Point Bucket

Policy Policy Policy

Access Point (Gateway

or Interface Endpoint)

- The VPC Endpoint Policy

must allow access to the

target bucket and Access

Point


### S3 Object Lambda

AWS Cloud

- Use AWS Lambda Functions to

change the object before it is

retrieved by the caller application Original

S3 Bucket

Object

- Only one S3 bucket is needed, on

E-Commerce

top of which we create S3 Access

Application

Point and S3 Object Lambda Access Supporting

S3 Access Point

Points. S3 Object Lambda Redacting

Access Point Lambda Function

- Use Cases:

Redacted

- Redacting personally identifiable Object

information for analytics or non-

Analytics

production environments.

Application

S3 Object Lambda Enriching

- Converting across data formats, such Access Point Lambda Function

as converting XML to JSON.

Enriched

- Resizing and watermarking images on Object

the fly using caller-specific details, such

Marketing

as the user who requested the object.

Application Customer Loyalty

Database


---

## CloudFront & Global Accelerator


### Amazon CloudFront

- Content Delivery Network (CDN)
- Improves read performance, content

is cached at the edge

- Improves users experience
- Hundreds of Points of Presence

globally (edge locations, caches)

- DDoS protection (because

worldwide), integration with Shield,

Source: https://aws.amazon.com/cloudfront/features/?nc=sn&loc=2

AWS Web Application Firewall


### CloudFront – Origins

- S3 bucket
- For distributing files and caching them at the edge
- For uploading files to S3 through CloudFront
- Secured using Origin Access Control (OAC)
- VPC Origin
- For applications hosted in VPC private subnets
- Private Application Load Balancer / Network Load Balancer / EC2 Instances
- Custom Origin (HTTP)
- S3 website (must first enable the bucket as a static S3 website)
- Any public HTTP backend you want (example: Public ALB)

### CloudFront at a high level

GET /beach.jpg?size=300x300 HTTP/1.1

User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)

Host: www.example.com

Accept-Encoding: gzip, deflate

Origin

Forward Request

to your Origin

CloudFront Edge Location or

Client

HTTP

Local Cache


| GET /beach.jpg?size=300x300 HTTP/1.1 User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT) Host: www.example.com Accept-Encoding: gzip, deflate |
| --- |
| Accept-Encoding: gzip, deflate |


### CloudFront – S3 as an Origin

AWS Cloud

Public www

Private AWS

Edge Private AWS Edge

Los Angeles Mumbai

Private AWS Private AWS

Origin (S3 bucket)

Public www

Edge Edge

São Paulo Melbourne

Origin Access Control

+ S3 bucket policy


### CloudFront vs S3 Cross Region Replication

- CloudFront:
- Global Edge network
- Files are cached for a TTL (maybe a day)
- Great for static content that must be available everywhere
- S3 Cross Region Replication:
- Must be setup for each region you want replication to happen
- Files are updated in near real-time
- Read only
- Great for dynamic content that needs to be available at low-latency in few

regions


### CloudFront – ALB or EC2 as an origin

Using VPC Origins

- Allows you to deliver content from your applications hosted in your

VPC private subnets (no need to expose them on the Internet)

- Deliver traffic to private:
- Application Load Balancer
- Network Load Balancer
- EC2 Instances VPC

Private Subnet

Application Load Balancer

Users

CloudFront

Origin Network Load Balancer

EC2 Instance

Edge Location


### CloudFront – ALB or EC2 as an origin

Using Public Network

Security group

Allow Public IP of Edge Locations

http://d7uri8nf7uskq.cloudfront.net/tools/list-cloudfront-ips

Edge Location EC2 Instances

Must be Public

Security group Security group

Allow Public IP of Allow Security Group

Edge Locations of Load Balancer

Edge Location EC2 Instances

Application Load Balancer

Public IPs Can be Private

Must be Public


### CloudFront Geo Restriction

- You can restrict who can access your distribution
- Allowlist: Allow your users to access your content only if they're in one of the

countries on a list of approved countries.

- Blocklist: Prevent your users from accessing your content if they're in one of the

countries on a list of banned countries.

- The “country” is determined using a 3rd party Geo-IP database
- Use case: Copyright Laws to control access to content

### CloudFront – Cache Invalidations

- In case you update the back-end

Invalidate

- /index.html

origin, CloudFront doesn’t know GET /index.html

- /images/*

about it and will only get the

refreshed content after the TTL has

CloudFront

expired

- However, you can force an entire or

invalidate

partial cache refresh (thus bypassing

the TTL) by performing a CloudFront

Edge Location Edge Location

Invalidation

Cache Cache

- You can invalidate all files (*) or a

special path (/images/*)

index.html /images/ index.html /images/

update files

S3 Bucket

(origin)


### Global users for our application

- You have deployed an

application and have global

users who want to access it

directly. hops

Europe

America

- They go over the public

internet, which can add a lot of

latency due to many hops

Public ALB

- We wish to go as fast as

possible through AWS network

Australia

India

to minimize latency


### Unicast IP vs Anycast IP

Client

- Unicast IP: one server holds one IP

address

12.34.56.78 98.76.54.32

- Anycast IP: all servers hold the same

Client

IP address and the client is routed to

the nearest one

12.34.56.78 12.34.56.78


### AWS Global Accelerator

- Leverage the AWS internal

network to route to your

application

- 2 Anycast IP are created for your Edge location Europe

America

application

- The Anycast IP send traffic directly

to Edge Locations

Private AWS

Public ALB

- The Edge locations send the traffic

Australia

to your application India


### AWS Global Accelerator

- Works with Elastic IP, EC2 instances, ALB, NLB, public or private
- Consistent Performance
- Intelligent routing to lowest latency and fast regional failover
- No issue with client cache (because the IP doesn’t change)
- Internal AWS network
- Health Checks
- Global Accelerator performs a health check of your applications
- Helps make your application global (failover less than 1 minute for unhealthy)
- Great for disaster recovery (thanks to the health checks)
- Security
- only 2 external IP need to be whitelisted
- DDoS protection thanks to AWS Shield

### AWS Global Accelerator vs CloudFront

- They both use the AWS global network and its edge locations around the world
- Both services integrate with AWS Shield for DDoS protection.
- CloudFront
- Improves performance for both cacheable content (such as images and videos)
- Dynamic content (such as API acceleration and dynamic site delivery)
- Content is served at the edge
- Global Accelerator
- Improves performance for a wide range of applications over TCP or UDP
- Proxying packets at the edge to applications running in one or more AWS Regions.
- Good fit for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP
- Good for HTTP use cases that require static IP addresses
- Good for HTTP use cases that required deterministic, fast regional failover

---

## AWS Storage Extras


### AWS Snowball

- Highly-secure, portable devices to collect and process data at the

edge, and migrate data into and out of AWS

- Helps migrate up to Petabytes of data

Device Compute Memory Storage (SSD)

Snowball Edge Storage Optimized 104 vCPUs 416 GB 210 TB

Snowball Edge Compute Optimized 104 vCPUs 416 GB 28 TB

Snowball Edge


| Device | Compute | Memory | Storage (SSD) |
| --- | --- | --- | --- |
| Snowball Edge Storage Optimized | 104 vCPUs | 416 GB | 210 TB |
| Snowball Edge Compute Optimized | 104 vCPUs | 416 GB | 28 TB |


### Data Migrations with Snowball

Challenges:

- Limited connectivity

Time to Transfer

- Limited bandwidth

100 Mbps 1Gbps 10Gbps

10 TB 12 days 30 hours 3 hours • High network cost

100 TB 124 days 12 days 30 hours • Shared bandwidth (can’t

maximize the line)

1 PB 3 years 124 days 12 days

- Connection stability

AWS Snowball: offline devices to perform data migrations

If it takes more than a week to transfer over the network, use Snowball devices!


|  | Time to Transfer |  |  |
| --- | --- | --- | --- |
|  | 100 Mbps | 1Gbps | 10Gbps |
| 10 TB | 12 days | 30 hours | 3 hours |
| 100 TB | 124 days | 12 days | 30 hours |
| 1 PB | 3 years | 124 days | 12 days |


### Diagrams

- Direct upload to S3:

www: 10Gbit/s

client Amazon S3

bucket

- With Snowball:

ship

AWS AWS import/ Amazon S3

client

Snowball Snowball export bucket


### What is Edge Computing?

- Process data while it’s being created on an edge location
- A truck on the road, a ship on the sea, a mining station underground...
- These locations may have limited internet and no access to computing power
- We setup a Snowball Edge device to do edge computing
- Snowball Edge Compute Optimized (dedicated for that use case) & Storage Optimized
- Run EC2 Instances or Lambda functions at the edge
- Use cases: preprocess data, machine learning, transcoding media

### Solution Architecture: Snowball into Glacier

- Snowball cannot import to Glacier directly
- You must use Amazon S3 first, in combination with an S3 lifecycle policy

import S3 lifecycle policy

Snowball Amazon S3 Amazon Glacier


### Amazon FSx – Over view

- Launch 3rd party high-performance file systems on AWS
- Fully managed service

FSx for

FSx for Lustre

NetApp ONTAP

FSx for Windows FSx for

File Server OpenZFS


### Amazon FSx for Windows (File Ser ver)

- FSx for Windows is a fully managed Windows file system share drive
- Supports SMB protocol & Windows NTFS
- Microsoft Active Directory integration, ACLs, user quotas
- Can be mounted on Linux EC2 instances
- Supports Microsoft's Distributed File System (DFS) Namespaces (group files across multiple FS)
- Scale up to 10s of GB/s, millions of IOPS, 100s PB of data
- Storage Options:
- SSD – latency sensitive workloads (databases, media processing, data analytics, …)
- HDD – broad spectrum of workloads (home directory, CMS, …)
- Can be accessed from your on-premises infrastructure (VPN or Direct Connect)
- Can be configured to be Multi-AZ (high availability)
- Data is backed-up daily to S3

### Amazon FSx for Lustre

- Lustre is a type of parallel distributed file system, for large-scale computing
- The name Lustre is derived from “Linux” and “cluster
- Machine Learning, High Performance Computing (HPC)
- Video Processing, Financial Modeling, Electronic Design Automation
- Scales up to 100s GB/s, millions of IOPS, sub-ms latencies
- Storage Options:
- SSD – low-latency, IOPS intensive workloads, small & random file operations
- HDD – throughput-intensive workloads, large & sequential file operations
- Seamless integration with S3
- Can “read S3” as a file system (through FSx)
- Can write the output of the computations back to S3 (through FSx)
- Can be used from on-premises servers (VPN or Direct Connect)

### FSx Lustre - File System Deployment Options

Region

- Scratch File System

Availability Zone 1 Availability Zone 2

- Temporary storage ENI

Compute Compute

instances instances

- Data is not replicated (doesn’t persist if file

server fails)

- High burst (6x faster, 200MBps per TiB)

FSx For Lustre S3 bucket

- Usage: short-term processing, optimize

(Scratch file system) (optional data repository)

costs

Region

- Persistent File System

Availability Zone 1 Availability Zone 2

- Long-term storage ENI

Compute Compute

- Data is replicated within same AZ instances instances
- Replace failed files within minutes
- Usage: long-term processing, sensitive data

FSx For Lustre S3 bucket

(Persistent file system) (optional data repository)


### Amazon FSx for NetApp ONTAP

- Managed NetApp ONTAP on AWS
- File System compatible with NFS, SMB, iSCSI protocol

Amazon FSx for

- Move workloads running on ONTAP or NAS to AWS

NetApp ONTAP FS

- Works with:
- Linux
- Windows

NFS, SMB, iSCSI

- MacOS
- VMware Cloud on AWS
- Amazon Workspaces & AppStream 2.0
- Amazon EC2, ECS and EKS
- Storage shrinks or grows automatically
- Snapshots, replication, low-cost, compression and data EC2 ECS EKS

de-duplication

- Point-in-time instantaneous cloning (helpful for testing

new workloads)

VMware Cloud Amazon Amazon On-premises

on AWS AppStream 2.0 WorkSpaces Server


### Amazon FSx for OpenZFS

- Managed OpenZFS file system on AWS
- File System compatible with NFS (v3, v4, v4.1, v4.2)

Amazon FSx

- Move workloads running on ZFS to AWS for OpenZFS
- Works with:
- Linux
- Windows NFS (v3, v4, v4.1, v4.2)
- MacOS
- VMware Cloud on AWS
- Amazon Workspaces & AppStream 2.0
- Amazon EC2, ECS and EKS
- Up to 1,000,000 IOPS with < 0.5ms latency

EC2 ECS EKS

- Snapshots, compression and low-cost
- Point-in-time instantaneous cloning (helpful for

testing new workloads)

VMware Cloud Amazon Amazon On-premises

on AWS AppStream 2.0 WorkSpaces Server


### Hybrid Cloud for Storage

- AWS is pushing for ”hybrid cloud”
- Part of your infrastructure is on the cloud
- Part of your infrastructure is on-premises
- This can be due to
- Long cloud migrations
- Security requirements
- Compliance requirements
- IT strategy
- S3 is a proprietary storage technology (unlike EFS / NFS), so how do

you expose the S3 data on-premises?

- AWS Storage Gateway!

### AWS Storage Cloud Native Options

Block File Object

Amazon EBS EC2 Instance Amazon EFS Amazon FSx Amazon S3 Amazon Glacier

Store


### AWS Storage Gateway

- Bridge between on-premises data and cloud data
- Use cases:
- disaster recovery
- backup & restore

Storage Gateway

- tiered storage
- on-premises cache & low-latency files access
- Types of Storage Gateway:
- S3 File Gateway
- Volume Gateway
- Tape Gateway

### Amazon S3 File Gateway

- Configured S3 buckets are accessible using the NFS and SMB protocol
- Most recently used data is cached in the file gateway
- Supports S3 Standard, S3 Standard IA, S3 One Zone A, S3 Intelligent Tiering
- Transition to S3 Glacier using a Lifecycle Policy
- Bucket access using IAM roles for each File Gateway
- SMB Protocol has integration with Active Directory (AD) for user authentication

Corporate AWS Cloud .

Data Center

S3 Standard Lifecycle

HTTPS

S3 Standard-IA policy

S3 One Zone-IA

NFS or SMB

S3 Intelligent-Tiering

S3 Glacier

Application S3 File

Server Gateway


### Volume Gateway

- Block storage using iSCSI protocol backed by S3
- Backed by EBS snapshots which can help restore on-premises volumes!
- Cached volumes: low latency access to most recent data
- Stored volumes: entire dataset is on premise, scheduled backups to S3

Corporate AWS Cloud

Data Center

Region

iSCSI HTTPS

Application Volume Gateway S3 Bucket Amazon EBS

Snapshots

Server


### Tape Gateway

- Some companies have backup processes using physical tapes (!)
- With Tape Gateway, companies use the same processes but, in the cloud
- Virtual Tape Library (VTL) backed by Amazon S3 and Glacier
- Back up data using existing tape-based processes (and iSCSI interface)
- Works with leading backup software vendors

Corporate AWS Cloud

Data Center

Region

Media

iSCSI Changer HTTPS

Virtual Tapes Archived Tapes

Tape Tape

Backup

stored in stored in

Drive Gateway

Server Amazon S3 Amazon Glacier


### AWS Storage Gateway

On-Premises AWS Cloud

NFS/SMB

Any S3 Storage Class

User/group file shares File Gateway Amazon S3

Including Glacier

local cache excluding Glacier &

Glacier Deep Archive

iSCSI Encryption in Transit

Internet or Direct Connect

Application Server Volume Gateway Amazon S3 AWS EBS

local cache Storage Gateway

Eject from backup application

iSCSI VTL

Tape Archive

Amazon S3

Backup Application Tape Gateway Tape Library Glacier &

Glacier Deep Archive

local cache

Gateway Deployment Options

VM(VMware, Hyper-V, KVM)


### AWS Transfer Family

- A fully-managed service for file transfers into and out of Amazon S3 or

Amazon EFS using the FTP protocol

- Supported Protocols
- AWS Transfer for FTP (File Transfer Protocol (FTP))
- AWS Transfer for FTPS (File Transfer Protocol over SSL (FTPS))
- AWS Transfer for SFTP (Secure File Transfer Protocol (SFTP))
- Managed infrastructure, Scalable, Reliable, Highly Available (multi-AZ)
- Pay per provisioned endpoint per hour + data transfers in GB
- Store and manage users’ credentials within the service
- Integrate with existing authentication systems (Microsoft Active Directory,

LDAP, Okta, Amazon Cognito, custom)

- Usage: sharing files, public datasets, CRM, ERP, …

### AWS Transfer Family

MS Active Directory

authenticate

LDAP

… AWS Transfer for SFTP

Amazon S3

IAM Role

AWS Transfer for FTPS

Users

Route 53

(FTP client)

(optional) AWS Transfer for FTP

(only within VPC)

Amazon EFS

AWS Transfer Family


### AWS DataSync

- Move large amount of data to and from
- On-premises / other cloud to AWS (NFS, SMB, HDFS, S3 API…) – needs agent
- AWS to AWS (different storage services) – no agent needed
- Can synchronize to:
- Amazon S3 (any storage classes – including Glacier)
- Amazon EFS
- Amazon FSx (Windows, Lustre, NetApp, OpenZFS...)
- Replication tasks can be scheduled hourly, daily, weekly
- File permissions and metadata are preserved (NFS POSIX, SMB…)
- One agent task can use 10 Gbps, can setup a bandwidth limit

### AWS DataSync

NFS / SMB to AWS (S3, EFS, FSx…)

Region

On-Premises

AWS Storage Resources

NFS or SMB TLS S3 Standard S3 Intelligent- S3 Standard-IA

Tiering

NFS or SMB AWS DataSync

AWS S3 One S3 Glacier S3 Glacier

Server Agent Zone-IA Deep Archive

DataSync

AWS Snowcone

(agent pre-installed)

AWS EFS Amazon FSx


### AWS DataSync

Transfer between AWS storage ser vices

Amazon S3 Amazon S3

Amazon EFS Amazon EFS

AWS DataSync

copy data and metadata

between AWS Storage Services

Amazon FSx Amazon FSx


### Storage Comparison

- S3: Object Storage
- S3 Glacier: Object Archival
- EBS volumes: Network storage for one EC2 instance at a time
- Instance Storage: Physical storage for your EC2 instance (high IOPS)
- EFS: Network File System for Linux instances, POSIX filesystem
- FSx for Windows: Network File System for Windows servers
- FSx for Lustre: High Performance Computing Linux file system
- FSx for NetApp ONTAP: High OS Compatibility
- FSx for OpenZFS: Managed ZFS file system
- Storage Gateway: S3 & FSx File Gateway, Volume Gateway (cache & stored), Tape Gateway
- Transfer Family: FTP, FTPS, SFTP interface on top of Amazon S3 or Amazon EFS
- DataSync: Schedule data sync from on-premises to AWS, or AWS to AWS
- Snowcone / Snowball / Snowmobile: to move large amount of data to the cloud, physically
- Database: for specific workloads, usually with indexing and querying

### AWS Integration & Messaging

SQS, SNS & Kinesis


### Section Introduction

- When we start deploying multiple applications, they will inevitably need

to communicate with one another

- There are two patterns of application communication

1) Synchronous communications 2) Asynchronous / Event based

(application to application) (application to queue to application)

Buying Shipping Buying Shipping

Queue

Service Service Service Service


### Section Introduction

- Synchronous between applications can be problematic if there are

sudden spikes of traffic

- What if you need to suddenly encode 1000 videos but usually it’s 10?
- In that case, it’s better to decouple your applications,
- using SQS: queue model
- using SNS: pub/sub model
- using Kinesis: real-time streaming model
- These services can scale independently from our application!

### Amazon SQS

What’s a queue?

Consumer

Producer

Consumer

Send messages

Producer Poll messages

Consumer

SQS Queue

Producer

Consumer


### Amazon SQS – Standard Queue

- Oldest offering (over 10 years old)
- Fully managed service, used to decouple applications
- Attributes:
- Unlimited throughput, unlimited number of messages in queue
- Default retention of messages: 4 days, maximum of 14 days
- Low latency (<10 ms on publish and receive)
- Limitation of 1,024 KB per message sent
- Can have duplicate messages (at least once delivery, occasionally)
- Can have out of order messages (best effort ordering)

### SQS – Producing Messages

- Produced to SQS using the SDK (SendMessage API)
- The message is persisted in SQS until a consumer deletes it
- Message retention: default 4 days, up to 14 days
- Example: send an order to be processed
- Order id

Sent to SQS

- Customer id
- Any attributes you want

Message

Up to 1024 KB

- SQS standard: unlimited throughput

### SQS – Consuming Messages

- Consumers (running on EC2 instances, servers, or AWS Lambda)…
- Poll SQS for messages (receive up to 10 messages at a time)
- Process the messages (example: insert the message into an RDS database)
- Delete the messages using the DeleteMessage API

Poll / Receive

messages

insert

Consumer

DeleteMessage


### SQS – Multiple EC2 Instances Consumers

- Consumers receive and process

messages in parallel

- At least once delivery

SQS Queue • Best-effort message ordering

- Consumers delete messages

after processing them

- We can scale consumers

horizontally to improve

throughput of processing

poll


### SQS with Auto Scaling Group (ASG)

Poll for messages

EC2 Instances

SQS Queue

Auto Scaling Group

scale

Alarm for breach

CloudWatch Metric – Queue Length CloudWatch Alarm

ApproximateNumberOfMessages


### SQS to decouple between application tiers

Back-end processing

Front-end web app

application

requests SendMessage ReceiveMessages

SQS Queue

(infinitely scalable)

Auto-Scaling

Auto-Scaling


### Amazon SQS - Security

- Encryption:
- In-flight encryption using HTTPS API
- At-rest encryption using KMS keys
- Client-side encryption if the client wants to perform encryption/decryption itself
- Access Controls: IAM policies to regulate access to the SQS API
- SQS Access Policies (similar to S3 bucket policies)
- Useful for cross-account access to SQS queues
- Useful for allowing other services (SNS, S3…) to write to an SQS queue

### SQS – Message Visibility Timeout

- After a message is polled by a consumer, it becomes invisible to other consumers
- By default, the “message visibility timeout” is 30 seconds
- That means the message has 30 seconds to be processed
- After the message visibility timeout is over, the message is “visible” in SQS

ReceiveMessage ReceiveMessage ReceiveMessage ReceiveMessage

Request Request Request Request

Visibility timeout

Not returned Not returned

Time

Message returned Message returned (again)


### SQS – Message Visibility Timeout

ReceiveMessage ReceiveMessage ReceiveMessage ReceiveMessage

Request Request Request Request

Visibility timeout

Not returned Not returned

Time

Message returned Message returned (again)

- If a message is not processed within the visibility timeout, it will be processed twice
- A consumer could call the ChangeMessageVisibility API to get more time
- If visibility timeout is high (hours), and consumer crashes, re-processing will take time
- If visibility timeout is too low (seconds), we may get duplicates

### Amazon SQS - Long Polling

message

- When a consumer requests messages from the

queue, it can optionally “wait” for messages to

arrive if there are none in the queue

- This is called Long Polling
- LongPolling decreases the number of API calls

made to SQS while increasing the efficiency and

SQS Queue

reducing latency of your application

- The wait time can be between 1 sec to 20 sec

(20 sec preferable)

- Long Polling is preferable to Short Polling

poll

- Long polling can be enabled at the queue level

or at the API level using WaitTimeSeconds

Consumer


### Amazon SQS – FIFO Queue

- FIFO = First In First Out (ordering of messages in the queue)

Send messages Poll messages

Producer Consumer

4 3 2 1 4 3 2 1

- Limited throughput: 300 msg/s without batching, 3000 msg/s with
- Exactly-once send capability (by removing duplicates using Deduplication ID)
- Messages are processed in order by the consumer
- Ordering by Message Group ID (all messages in the same group are ordered)
- mandatory parameter

### SQS with Auto Scaling Group (ASG)

Poll for messages

EC2 Instances

SQS Queue

Auto Scaling Group

scale

Alarm for breach

CloudWatch Metric – Queue Length CloudWatch Alarm

ApproximateNumberOfMessages


### If the load is too big,

some transactions may be lost

Amazon RDS

Application

Insert

transactions

requests

Amazon Aurora

Amazon DynamoDB

Auto-Scaling


### SQS as a buffer to database writes

Dequeue message

Enqueue message

requests SendMessage ReceiveMessages insert

SQS Queue

(infinitely scalable)

Auto-Scaling

Auto-Scaling


### SQS to decouple between application tiers

Back-end processing

Front-end web app

application

requests SendMessage ReceiveMessages

SQS Queue

(infinitely scalable)

Auto-Scaling

Auto-Scaling


### Amazon SNS

- What if you want to send one message to many receivers?

Direct Pub / Sub

Email

Email

integration

notification

notification

Fraud

Fraud

Service

Service

Buying Buying

Service Service

Shipping

Shipping

Service

SNS Topic Service

SQS Queue

SQS Queue


### Amazon SNS

- The “event producer” only sends message to one SNS topic
- As many “event receivers” (subscriptions) as we want to listen to the SNS topic notifications
- Each subscriber to the topic will get all the messages (note: new feature to filter messages)
- Up to 12,500,000 subscriptions per topic
- 100,000 topics limit

Subscribers

publish SQS Lambda Kinesis Data

Firehose

Emails SMS & HTTP(S)

Mobile Notifications Endpoints


### SNS integrates with a lot of AWS ser vices

- Many AWS services can send data directly to SNS for notifications

CloudWatch Alarms AWS Budgets Lambda

publish

Auto Scaling Group S3 Bucket DynamoDB

(Notifications) (Events)

CloudFormation AWS DMS RDS Events

(State Changes) (New Replic)


### Amazon SNS – How to publish

- Topic Publish (using the SDK)
- Create a topic
- Create a subscription (or many)
- Publish to the topic
- Direct Publish (for mobile apps SDK)
- Create a platform application
- Create a platform endpoint
- Publish to the platform endpoint
- Works with Google GCM, Apple APNS, Amazon ADM…

### Amazon SNS – Security

- Encryption:
- In-flight encryption using HTTPS API
- At-rest encryption using KMS keys
- Client-side encryption if the client wants to perform encryption/decryption itself
- Access Controls: IAM policies to regulate access to the SNS API
- SNS Access Policies (similar to S3 bucket policies)
- Useful for cross-account access to SNS topics
- Useful for allowing other services ( S3…) to write to an SNS topic

### SNS + SQS: Fan Out

SQS Queue

Fraud

Service

Buying

Service

Shipping

SNS Topic Service

SQS Queue

- Push once in SNS, receive in all SQS queues that are subscribers
- Fully decoupled, no data loss
- SQS allows for: data persistence, delayed processing and retries of work
- Ability to add more SQS subscribers over time
- Make sure your SQS queue access policy allows for SNS to write
- Cross-Region Delivery: works with SQS Queues in other regions

### Application: S3 Events to multiple queues

- For the same combination of: event type (e.g. object create) and prefix

(e.g. images/) you can only have one S3 Event rule

- If you want to send the same S3 event to many SQS queues, use fan-out

SQS Queues

Fan-out

events

S3 Object

created…

SNS Topic

Amazon S3

Lambda Function


### Application: SNS to Amazon S3 through

Kinesis Data Firehose

- SNS can send to Kinesis and therefore we can have the following

solutions architecture:

Buying

Amazon S3

Service

SNS Topic Kinesis Data

Firehose

Any supported KDF

Destination


### Amazon SNS – FIFO Topic

- FIFO = First In First Out (ordering of messages in the topic)

Receive messages

Send messages

Subscribers

Producer

SQS FIFO

4 3 2 1 4 3 2 1

- Similar features as SQS FIFO:
- Ordering by Message Group ID (all messages in the same group are ordered)
- Deduplication using a Deduplication ID or Content Based Deduplication
- Can have SQS Standard and FIFO queues as subscribers
- Limited throughput (same throughput as SQS FIFO)

### SNS FIFO + SQS FIFO: Fan Out

- In case you need fan out + ordering + deduplication

SQS FIFO Queue

Fraud

Service

Buying

Service

Shipping

SNS FIFO Topic Service

SQS FIFO Queue


### SNS – Message Filtering

- JSON policy used to filter messages sent to SNS topic’s subscriptions
- If a subscription doesn’t have a filter policy, it receives every message

Filter Policy

SQS Queue

State: Placed

(Placed orders)

SQS Queue

(Cancelled orders)

Filter Policy

New transaction Email Subscription

Buying State: Cancelled

(Cancelled orders)

Service

Order: 1036

Filter Policy

Product: Pencil SNS Topic

State: Declined

Qty: 4 SQS Queue

State: Placed

(Declined orders)

SQS Queue

(All)


### Amazon Kinesis Data Streams

- Collect and store streaming data in real-time

Real-time data

Consumers

Application

Amazon Kinesis

Click Streams

Producers Data Streams

Lambda

Applications

Amazon

IoT devices

Data Firehose

Kinesis Agent

Managed

Metrics & Logs Service for

Apache Flink


### Kinesis Data Streams

- Retention between up to 365 days
- Ability to reprocess (replay) data by consumers
- Data can’t be deleted from Kinesis (until it expires)
- Data up to 10MiB (typical use case is lot of “small” real-time data)
- Data ordering guarantee for data with the same “Partition ID”
- At-rest KMS encryption, in-flight HTTPS encryption
- Kinesis Producer Library (KPL) to write an optimized producer application
- Kinesis Client Library (KCL) to write an optimized consumer application

### Kinesis Data Streams – Capacity Modes

- Provisioned mode:
- Choose number of shards
- Each shard gets 1MB/s in (or 1000 records per second)
- Each shard gets 2MB/s out
- Scale manually to increase or decrease the number of shards
- You pay per shard provisioned per hour
- On-demand mode:
- No need to provision or manage the capacity
- Default capacity provisioned (4 MB/s in or 4000 records per second)
- Scales automatically based on observed throughput peak during the last 30 days
- Pay per stream per hour & data in/out per GB

### Amazon Data Firehose

3rd-party Partner Destinations

Lambda

function

Datadog

Applications

Data

Kinesis

transformation

Data Streams AWS Destinations

Amazon S3

Client Record

Up to 1 MB

Amazon Redshift

Batch writes

Amazon

CloudWatch

Amazon

(Logs & Events)

Amazon OpenSearch

Data Firehose

All or Failed data

Kinesis Agent

Custom Destinations

AWS IoT

S3 backup bucket

Producers HTTP Endpoint


### Amazon Data Firehose

- Note: used to be called “Kinesis Data Firehose”
- Fully Managed Service
- Amazon Redshift / Amazon S3 / Amazon OpenSearch Service
- 3rd party: Splunk / MongoDB / Datadog / NewRelic / …
- Custom HTTP Endpoint
- Automatic scaling, serverless, pay for what you use
- Near Real-Time with buffering capability based on size / time
- Supports CSV, JSON, Parquet, Avro, Raw Text, Binary data
- Conversions to Parquet / ORC, compressions with gzip / snappy
- Custom data transformations using AWS Lambda (ex: CSV to JSON)

### Kinesis Data Streams vs Amazon Data Firehose

Kinesis Data Streams Amazon Data Firehose

- Streaming data collection • Load streaming data into S3 / Redshift /

OpenSearch / 3rd party / custom HTTP

- Producer & Consumer code
- Fully managed
- Real-time
- Near real-time
- Provisioned / On-Demand mode
- Automatic scaling
- Data storage up to 365 days
- No data storage
- Replay Capability
- Doesn’t support replay capability

### SQS vs SNS vs Kinesis

SQS: SNS: Kinesis:

- Push data to many
- Consumer “pull data” • Standard: pull data

subscribers

- 2 MB per shard
- Data is deleted after being
- Up to 12,500,000 subscribers

consumed • Enhanced-fan out: push data

- Data is not persisted (lost if
- 2 MB per shard per consumer
- Can have as many workers not delivered)

(consumers) as we want • Possibility to replay data

- Pub/Sub
- No need to provision • Meant for real-time big data,
- Up to 100,000 topics

throughput analytics and ETL

- No need to provision
- Ordering guarantees only on throughput • Ordering at the shard level

FIFO queues

- Integrates with SQS for fan-
- Data expires after X days

out architecture pattern

- Individual message delay
- Provisioned mode or on-

capability • FIFO capability for SQS FIFO

demand capacity mode


### Amazon MQ

- SQS, SNS are “cloud-native” services: proprietary protocols from AWS
- Traditional applications running from on-premises may use open protocols

such as: MQTT, AMQP, STOMP, Openwire, WSS

- When migrating to the cloud, instead of re-engineering the application to use

SQS and SNS, we can use Amazon MQ

- Amazon MQ is a managed message broker service for
- Amazon MQ doesn’t “scale” as much as SQS / SNS
- Amazon MQ runs on servers, can run in Multi-AZ with failover
- Amazon MQ has both queue feature (~SQS) and topic features (~SNS)

### Amazon MQ – High Availability

Region

(us-east-1)

ACTIVE Availability Zone

(us-east-1a)

Amazon MQ Broker

Amazon EFS

(storage)

Client STANDBY Availability Zone

failover (us-east-1b)

Amazon MQ Broker


---

## Containers on AWS


### What is Docker?

- Docker is a software development platform to deploy apps
- Apps are packaged in containers that can be run on any OS
- Apps run the same, regardless of where they’re run
- Any machine
- No compatibility issues
- Predictable behavior
- Less work
- Easier to maintain and deploy
- Works with any language, any OS, any technology
- Use cases: microservices architecture, lift-and-shift apps from on-

premises to the AWS cloud, …


### Docker on an OS

Server (e.g., EC2 instance)


### Where are Docker images stored?

- Docker images are stored in Docker Repositories
- Docker Hub (https://hub.docker.com)
- Public repository
- Find base images for many technologies or OS (e.g., Ubuntu, MySQL, …)
- Amazon ECR (Amazon Elastic Container Registry)
- Private repository
- Public repository (Amazon ECR Public Gallery https://gallery.ecr.aws)

### Docker vs. Vir tual Machines

- Docker is ”sort of ” a virtualization technology, but not exactly
- Resources are shared with the host => many containers on one server

Apps Apps Apps

Guest OS Guest OS Guest OS

(VM) (VM) (VM)

Hypervisor Docker Daemon

Host OS Host OS (EC2 Instance)

Infrastructure Infrastructure


### Getting Star ted with Docker

Build Run

container

Dockerfile

image

Push Pull

Docker Repository

Amazon


### Docker Containers Management on AWS

- Amazon Elastic Container Service (Amazon ECS)

Amazon ECS

- Amazon’s own container platform
- Amazon Elastic Kubernetes Service (Amazon EKS)

Amazon EKS

- Amazon’s managed Kubernetes (open source)
- AWS Fargate

AWS Fargate

- Amazon’s own Serverless container platform
- Works with ECS and with EKS
- Amazon ECR:

Amazon ECR

- Store container images

### Amazon ECS - EC2 Launch Type

Amazon ECS / ECS Cluster

- ECS = Elastic Container Service

New Docker

- Launch Docker containers on AWS =

Container

Launch ECS Tasks on ECS Clusters

- EC2 Launch Type: you must provision

& maintain the infrastructure (the EC2

EC2 Instance EC2 Instance EC2 Instance

instances)

- Each EC2 Instance must run the ECS

Agent to register in the ECS Cluster

- AWS takes care of starting / stopping

containers

ECS Agent ECS Agent ECS Agent


### Amazon ECS – Fargate Launch Type

- Launch Docker containers on AWS

New Docker

- You do not provision the infrastructure Container

(no EC2 instances to manage)

- It’s all Serverless!

AWS Fargate / ECS Cluster

- You just create task definitions
- AWS just runs ECS Tasks for you based

on the CPU / RAM you need

- To scale, just increase the number of

tasks. Simple - no more EC2 instances


### Amazon ECS – IAM Roles for ECS

EC2 Instance Profile

- EC2 Instance Profile (EC2 Launch Type only):

EC2 Instance

- Used by the ECS agent
- Makes API calls to ECS service
- Send container logs to CloudWatch Logs
- Pull Docker image from ECR ECS Agent CloudWatch

Logs

- Reference sensitive data in Secrets Manager or

SSM Parameter Store ECS Task A Role

Task A S3

- ECS Task Role:
- Allows each task to have a specific role ECS Task B Role
- Use different roles for the different ECS Services

Task B DynamoDB

you run

- Task Role is defined in the task definition

| ECS Ta | sk A Role |
| --- | --- |


| ECS Ta | sk B Role |
| --- | --- |


### Amazon ECS – Load Balancer Integrations

- Application Load Balancer supported

EC2 Instance

and works for most use cases

ECS Task

- Network Load Balancer recommended

ECS Task

only for high throughput / high

performance use cases, or to pair it with

80/443

AWS Private Link

EC2 Instance

Users

Application

ECS Task

Load Balancer

- Classic Load Balancer supported but

not recommended (no advanced ECS Task

features – no Fargate)

ECS Cluster


|  |  |
| --- | --- |
|  |  |
|  |  |


### Amazon ECS – Data Volumes (EFS)

- Mount EFS file systems onto ECS tasks
- Works for both EC2 and Fargate launch types

EC2 Instance Fargate

- Tasks running in any AZ will share the same data

in the EFS file system

- Fargate + EFS = Serverless
- Use cases: persistent multi-AZ shared storage for

mount mount

your containers

ECS Cluster

- Note:

Amazon EFS

- Amazon S3 cannot be mounted as a file system

File System


### ECS Ser vice Auto Scaling

- Automatically increase/decrease the desired number of ECS tasks
- Amazon ECS Auto Scaling uses AWS Application Auto Scaling
- ECS Service Average CPU Utilization
- ECS Service Average Memory Utilization - Scale on RAM
- ALB Request Count Per Target – metric coming from the ALB
- Target Tracking – scale based on target value for a specific CloudWatch metric
- Step Scaling – scale based on a specified CloudWatch Alarm
- Scheduled Scaling – scale based on a specified date/time (predictable changes)
- ECS Service Auto Scaling (task level) ≠ EC2 Auto Scaling (EC2 instance level)
- Fargate Auto Scaling is much easier to setup (because Serverless)

### EC2 Launch Type – Auto Scaling EC2 Instances

- Accommodate ECS Service Scaling by adding underlying EC2 Instances
- Auto Scaling Group Scaling
- Scale your ASG based on CPU Utilization
- Add EC2 instances over time
- ECS Cluster Capacity Provider
- Used to automatically provision and scale the infrastructure for your ECS Tasks
- Capacity Provider paired with an Auto Scaling Group
- Add EC2 Instances when you’re missing capacity (CPU, RAM…)

### ECS Scaling – Ser vice CPU Usage Example

Usage

Task 1 Task 3

(new)

Task 2

Service A

Auto Scaling

Auto Scaling Group e

Scale ECS Capacity Providers

(optional)

CloudWatch Metric Trigger

(ECS Service CPU Usage)

CloudWatch Alarm


### ECS tasks invoked by Event Bridge

Region

Upload object

AWS Fargate

Client t

S3 Bucket b

Task

(new)

ECS Task Role

Event

(Access S3 & DynamoDB) Save result

Task

ule:

Amazon

DynamoDB

Amazon ECS Cluster

Amazon

EventBridge


| ECS Task Role (Access S3 & Dynam | oD | B) |  |
| --- | --- | --- | --- |


### ECS tasks invoked by Event Bridge Schedule

AWS Fargate

Task

(new) ECS Task Role

Every 1 hour Access S3

Rule: Run ECS Task Batch Processing

Amazon S3

Amazon

EventBridge

Amazon ECS Cluster


### ECS – SQS Queue Example

Task 1 Task 3

Messages Poll for messages

SQS Queue

Task 2

Service A

ECS Service Auto Scaling


### ECS – Intercept Stopped Tasks using EventBridge

ECS Task

event

trigger email

exited

EventBridge SNS Administrator

Containers

Event Pattern


### Amazon ECR

ECR Repository

- ECR = Elastic Container Registry

Docker Docker

- Store and manage Docker images on AWS

Image A Image B

- Private and Public repository (Amazon ECR

Public Gallery https://gallery.ecr.aws)

pull pull

- Fully integrated with ECS, backed by Amazon S3 IAM Role
- Access is controlled through IAM (permission

EC2 Instance

errors => policy)

- Supports image vulnerability scanning, versioning,

image tags, image lifecycle, …

ECS Cluster


### Amazon EKS Over view

- Amazon EKS = Amazon Elastic Kubernetes Service
- It is a way to launch managed Kubernetes clusters on AWS
- Kubernetes is an open-source system for automatic deployment, scaling and

management of containerized (usually Docker) application

- It’s an alternative to ECS, similar goal but different API
- EKS supports EC2 if you want to deploy worker nodes or Fargate to deploy

serverless containers

- Use case: if your company is already using Kubernetes on-premises or in

another cloud, and wants to migrate to AWS using Kubernetes

- Kubernetes is cloud-agnostic (can be used in any cloud – Azure, GCP…)
- For multiple regions, deploy one EKS cluster per region
- Collect logs and metrics using CloudWatch Container Insights

### Amazon EKS - Diagram

AWS Cloud

Availability Zone 1 Availability Zone 2 Availability Zone 3

Public subnet 1 Public subnet 2 Public subnet 3

Public

Service LB

NGW NGW NGW

ELB ELB ELB

Private subnet 1 Private subnet 2 Private subnet 3 EKS

Private

Service LB

EKS node EKS node EKS node

Auto Scaling Group

EKS Pods EKS Pods EKS Pods

EKS Worker Nodes


| Availability Zone 1 |  |  |  | Availability Zone 2 |  |  |  | Availability Zone 3 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Public subnet 1 NGW ELB Private subnet 1 EKS node Auto S EKS Pods |  |  |  | Public subnet 2 EKS Public Service LB NGW ELB Private subnet 2 |  |  |  | Public subnet 3 NGW ELB Private subnet 3 EKS Private Service LB ELB EKS node EKS Pods |  |
|  |  |  |  |  | Private subnet 2 |  |  |  |  |
|  | EKS node Auto EKS Pods | S | calin | g G | EKS node roup EKS Pods |  |  |  | EKS node EKS Pods |
|  |  |  |  |  |  |  |  |  |  |


### Amazon EKS – Node Types

- Managed Node Groups
- Creates and manages Nodes (EC2 instances) for you
- Nodes are part of an ASG managed by EKS
- Supports On-Demand or Spot Instances
- Self-Managed Nodes
- Nodes created by you and registered to the EKS cluster and managed by an ASG
- You can use prebuilt AMI - Amazon EKS Optimized AMI
- Supports On-Demand or Spot Instances
- AWS Fargate
- No maintenance required; no nodes managed

### Amazon EKS – Data Volumes

- Need to specify StorageClass manifest on your EKS cluster
- Leverages a Container Storage Interface (CSI) compliant driver
- Support for…
- Amazon EBS
- Amazon EFS (works with Fargate)
- Amazon FSx for Lustre
- Amazon FSx for NetApp ONTAP

### AWS App Runner

- Fully managed service that makes it easy to deploy web

applications and APIs at scale Container Source

Image (Docker) Code

- No infrastructure experience required
- Start with your source code or container image

Configure Settings

- Automatically builds and deploy the web app

vCPU, RAM,

Auto Scaling,

- Automatic scaling, highly available, load balancer, encryption

Health Check

- VPC access support
- Connect to database, cache, and message queue services

Create & Deploy

- Use cases: web apps, APIs, microservices, rapid production

deployments

Access using URL


### AWS App2Container (A2C)

- CLI tool for migrating and modernizing Java and .NET web apps into

Docker Containers

- Lift-and-shift your apps running in on-premises bare metal, virtual

machines, or in any Cloud to AWS

- Accelerate modernization, no code changes, migrate legacy apps…
- Generates CloudFormation templates (compute, network…)
- Register generated Docker containers to ECR
- Deploy to ECS, EKS, or App Runner
- Supports pre-built CI/CD pipelines

### AWS App2Container (A2C)

Amazon ECR

(store image)

Discover & Analyze Extract & Containerize Create Deployment Deploy to AWS

Amazon ECS

create app inventory extract an app with Artifacts store Docker image in ECR,

(deploy)

and analyze runtime dependencies and create and deploy to ECS, EKS, or

generate ECS Task and

dependencies a Docker image App Runner

EKS Pod definitions, and

create CI/CD pipelines, and

Amazon EKS

other infrastructure

(deploy)

App Runner

(deploy)

CloudFormation

Template


---

## Ser verless Over view


### What’s ser verless?

- Serverless is a new paradigm in which the developers don’t have to

manage servers anymore…

- They just deploy code
- They just deploy… functions !
- Initially... Serverless == FaaS (Function as a Service)
- Serverless was pioneered by AWS Lambda but now also includes

anything that’s managed: “databases, messaging, storage, etc.”

- Serverless does not mean there are no servers…

it means you just don’t manage / provision / see them


### Ser verless in AWS

e • AWS Lambda t

- DynamoDB c
- AWS Cognito
- AWS API Gateway
- Amazon S3
- AWS SNS & SQS
- AWS Kinesis Data Firehose
- Aurora Serverless
- Step Functions
- Fargate

TSER

Users

S3 bucket API Gateway

Cognito

Lambda

DynamoDB


### Why AWS Lambda

- Virtual Servers in the Cloud
- Limited by RAM and CPU
- Continuously running
- Scaling means intervention to add / remove servers

Amazon EC2

- Virtual functions – no servers to manage!
- Limited by time - short executions
- Run on-demand

Amazon Lambda • Scaling is automated!


### Benefits of AWS Lambda

- Easy Pricing:
- Pay per request and compute time
- Free tier of 1,000,000 AWS Lambda requests and 400,000 GBs of compute time
- Integrated with the whole AWS suite of services
- Integrated with many programming languages
- Easy monitoring through AWS CloudWatch
- Easy to get more resources per functions (up to 10GB of RAM!)
- Increasing RAM will also improve CPU and network!

### AWS Lambda language suppor t

- Node.js (JavaScript)
- Python
- Java
- C# (.NET Core) / Powershell
- Ruby
- Custom Runtime API (community supported, example Rust or Golang)
- Lambda Container Image
- The container image must implement the Lambda Runtime API
- ECS / Fargate is preferred for running arbitrary Docker images

### AWS Lambda Integrations

Main ones

API Gateway Kinesis DynamoDB S3 CloudFront

CloudWatch Events CloudWatch Logs SNS SQS Cognito

EventBridge


### Example: Ser verless Thumbnail creation

New thumbnail in S3

trigger

h Image name

AWS Lambda Function Image size

New image in S3

Creates a Thumbnail

Creation date

etc…

Metadata in DynamoDB


### Example: Ser verless CRON Job

Trigger

Every 1 hour

CloudWatch Events

AWS Lambda Function

EventBridge

Perform a task


### AWS Lambda Pricing: example

- You can find overall pricing information here:

https://aws.amazon.com/lambda/pricing/

- Pay per calls:
- First 1,000,000 requests are free
- $0.20 per 1 million requests thereafter ($0.0000002 per request)
- Pay per duration: (in increment of 1 ms)
- 400,000 GB-seconds of compute time per month for FREE
- == 400,000 seconds if function is 1GB RAM
- == 3,200,000 seconds if function is 128 MB RAM
- After that $1.00 for 600,000 GB-seconds
- It is usually very cheap to run AWS Lambda so it’s very popular

### AWS Lambda Limits to Know - per region

- Execution:
- Memory allocation: 128 MB – 10GB (1 MB increments)
- Maximum execution time: 900 seconds (15 minutes)
- Environment variables (4 KB)
- Disk capacity in the “function container” (in /tmp): 512 MB to 10GB
- Concurrency executions: 1000 (can be increased)
- Deployment:
- Lambda function deployment size (compressed .zip): 50 MB
- Size of uncompressed deployment (code + dependencies): 250 MB
- Can use the /tmp directory to load other files at startup
- Size of environment variables: 4 KB

### Lambda Concurrency and Throttling

- Concurrency limit: up to 1000 concurrent executions
- Can set a “reserved concurrency” at the function level (=limit)
- Each invocation over the concurrency limit will trigger a “Throttle”
- Throttle behavior:
- If synchronous invocation => return ThrottleError - 429
- If asynchronous invocation => retry automatically and then go to DLQ
- If you need a higher limit, open a support ticket

### Lambda Concurrency Issue

- If you don’t reserve (=limit) concurrency, the following can happen:

1000 concurrent

executions

Many users

Application Load Balancer

THROTTLE!

Few users

API Gateway

THROTTLE!

SDK / CLI


### Concurrency and Asynchronous Invocations

- If the function doesn't have enough

concurrency available to process all

events, additional requests are

throttled.

New file event

- For throttling errors (429) and

system errors (500-series), Lambda

New file event

returns the event to the queue and

attempts to run the function again

S3 bucket

for up to 6 hours.

- The retry interval increases

New file event

exponentially from 1 second after

the first attempt to a maximum of

5 minutes.


### Cold Star ts & Provisioned Concurrency

- Cold Start:
- New instance => code is loaded and code outside the handler run (init)
- If the init is large (code, dependencies, SDK…) this process can take some time.
- First request served by new instances has higher latency than the rest
- Provisioned Concurrency:
- Concurrency is allocated before the function is invoked (in advance)
- So the cold start never happens and all invocations have low latency
- Application Auto Scaling can manage concurrency (schedule or target utilization)
- Note:
- Note: cold starts in VPC have been dramatically reduced in Oct & Nov 2019
- https://aws.amazon.com/blogs/compute/announcing-improved-vpc-networking-for-aws-lambda-functions/

### Reser ved and Provisioned Concurrency

https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html


### Lambda SnapStar t

SnapStart SnapStart

- Improves your Lambda functions performance enabled disabled

up to 10x at no extra cost for Java, Python & .NET

invoke invoke

- When enabled, function is invoked from a pre-

initialized state (no function initialization from

Lambda Lambda

scratch)

function is

- When you publish a new version: pre-initialized

Init

- Lambda initializes your function
- Takes a snapshot of memory and disk state of the Invoke Invoke

initialized function

Shutdown Shutdown

- Snapshot is cached for low-latency access

Lambda Invocation

Lifecycle Phases


### Customization At The Edge

- Many modern applications execute some form of the logic at the edge
- Edge Function:
- A code that you write and attach to CloudFront distributions
- Runs close to your users to minimize latency
- CloudFront provides two types: CloudFront Functions & Lambda@Edge
- You don’t have to manage any servers, deployed globally
- Use case: customize the CDN content
- Pay only for what you use
- Fully serverless

### CloudFront Functions & Lambda@Edge

Use Cases

- Website Security and Privacy
- Dynamic Web Application at the Edge
- Search Engine Optimization (SEO)
- Intelligently Route Across Origins and Data Centers
- Bot Mitigation at the Edge
- Real-time Image Transformation
- A/B Testing
- User Authentication and Authorization
- User Prioritization
- User Tracking and Analytics

### CloudFront Functions

Client

- Lightweight functions written in JavaScript
- For high-scale, latency-sensitive CDN customizations

Viewer Viewer

Request Response

- Sub-ms startup times, millions of requests/second
- Used to change Viewer requests and responses:
- Viewer Request: after CloudFront receives a request from a

CloudFront

viewer

- Viewer Response: before CloudFront forwards the response

Origin Origin

to the viewer

Request Response

- Native feature of CloudFront (manage code entirely

within CloudFront)

Origin


### Lambda@Edge

Client

- Lambda functions written in NodeJS or Python
- Scales to 1000s of requests/second

Viewer Viewer

Request Response

- Used to change CloudFront requests and responses:
- Viewer Request – after CloudFront receives a request from a

viewer

- Origin Request – before CloudFront forwards the request to the

origin

CloudFront

- Origin Response – after CloudFront receives the response from the

origin

Origin Origin

- Viewer Response – before CloudFront forwards the response to

Request Response

the viewer

- Author your functions in one AWS Region (us-east-1), then

CloudFront replicates to its locations

Origin


### CloudFront Functions vs. Lambda@Edge

CloudFront Functions Lambda@Edge

Runtime Support JavaScript Node.js, Python

# of Requests Millions of requests per second Thousands of requests per second

CloudFront Triggers - Viewer Request/Response - Viewer Request/Response

- Origin Request/Response

Max. Execution Time < 1 ms 5 – 10 seconds

Max. Memory 2 MB 128 MB up to 10 GB

Total Package Size 10 KB 1 MB – 50 MB

Network Access, File System Access No Yes

Access to the Request Body No Yes

Pricing Free tier available, 1/6th price of @Edge No free tier, charged per request & duration


|  | CloudFront Functions | Lambda@Edge |
| --- | --- | --- |
| Runtime Support | JavaScript | Node.js, Python |
| # of Requests | Millions of requests per second | Thousands of requests per second |
| CloudFront Triggers | - Viewer Request/Response | - Viewer Request/Response - Origin Request/Response |
| Max. Execution Time | < 1 ms | 5 – 10 seconds |
| Max. Memory | 2 MB | 128 MB up to 10 GB |
| Total Package Size | 10 KB | 1 MB – 50 MB |
| Network Access, File System Access | No | Yes |
| Access to the Request Body | No | Yes |
| Pricing | Free tier available, 1/6th price of @Edge | No free tier, charged per request & duration |


### CloudFront Functions vs. Lambda@Edge - Use Cases

CloudFront Functions

Lambda@Edge

- Cache key normalization
- Longer execution time (several ms)
- Transform request attributes (headers,
- Adjustable CPU or memory

cookies, query strings, URL) to create an

optimal Cache Key

- Your code depends on a 3rd
- Header manipulation

libraries (e.g., AWS SDK to access

- Insert/modify/delete HTTP headers in the

other AWS services)

request or response

- URL rewrites or redirects • Network access to use external
- Request authentication & authorization services for processing
- Create and validate user-generated
- File system access or access to the

tokens (e.g., JWT) to allow/deny requests

body of HTTP requests


### Lambda by default

Default Lambda Deployment

- By default, your Lambda function is

launched outside your own VPC (in AWS Cloud

an AWS-owned VPC)

Public

- Therefore, it cannot access resources

works

DynamoDB

in your VPC (RDS, ElastiCache,

internal ELB…)

VPC & Private Subnet

Not working

Private RDS


### Lambda in VPC

Lambda Function

- You must define the VPC ID, the

Private subnet

Subnets and the Security Groups

- Lambda will create an ENI (Elastic

Lambda Security group

Network Interface) in your subnets

Elastic Network

Interface (ENI)

RDS Security group

Amazon RDS

In VPC


### Lambda with RDS Proxy

Lambda functions

- If Lambda functions directly access your

database, they may open too many

connections under high load

- RDS Proxy
- Improve scalability by pooling and sharing DB

connections

Private subnet

- Improve availability by reducing by 66% the

failover time and preserving connections RDS Proxy

- Improve security by enforcing IAM

authentication and storing credentials in

Secrets Manager

- The Lambda function must be deployed in

RDS DB

your VPC, because RDS Proxy is never

Instance

publicly accessible


### Invoking Lambda from RDS & Aurora

User

- Invoke Lambda functions from within your DB instance

register

- Allows you to process data events from within a database (INSERT)

RDS DB

- Supported for RDS for PostgreSQL and Aurora MySQL

Instance

- Must allow outbound traffic to your Lambda function

Permissions

invoke

from within your DB instance (Public, NAT GW, VPC

Endpoints)

Lambda

- DB instance must have the required permissions to

function

invoke the Lambda function (Lambda Resource-based

send Email

Policy & IAM Policy)

Amazon SES


### RDS Event Notifications

RDS DB

Instance

- Notifications that tells information about the DB

instance itself (created, stopped, start, …)

- You don’t have any information about the data itself
- Subscribe to the following event categories: DB

instance, DB snapshot, DB Parameter Group, DB

Security Group, RDS Proxy, Custom Engine Version

SNS EventBridge

- Near real-time events (up to 5 minutes)
- Send notifications to SNS or subscribe to events

using EventBridge

Lambda Lambda

Queue

function function


### Amazon DynamoDB

- Fully managed, highly available with replication across multiple AZs
- NoSQL database - not a relational database - with transaction support
- Scales to massive workloads, distributed database
- Millions of requests per seconds, trillions of row, 100s of TB of storage
- Fast and consistent in performance (single-digit millisecond)
- Integrated with IAM for security, authorization and administration
- Low cost and auto-scaling capabilities
- No maintenance or patching, always available
- Standard & Infrequent Access (IA) Table Class

### DynamoDB - Basics

- DynamoDB is made of Tables
- Each table has a Primary Key (must be decided at creation time)
- Each table can have an infinite number of items (= rows)
- Each item has attributes (can be added over time – can be null)
- Maximum size of an item is 400KB
- Data types supported are:
- Scalar Types – String, Number, Binary, Boolean, Null
- Document Types – List, Map
- Set Types – String Set, Number Set, Binary Set
- Therefore, in DynamoDB you can rapidly evolve schemas

### DynamoDB – Table example

Primary Key Attributes

Partition Key Sort Key

User_ID Game_ID Score Result

7791a3d6-… 4421 92 Win

873e0634-… 1894 14 Lose

873e0634-… 4521 77 Win


### DynamoDB – Read/Write Capacity Modes

- Control how you manage your table’s capacity (read/write throughput)
- Provisioned Mode (default)
- You specify the number of reads/writes per second
- You need to plan capacity beforehand
- Pay for provisioned Read Capacity Units (RCU) & Write Capacity Units (WCU)
- Possibility to add auto-scaling mode for RCU & WCU
- On-Demand Mode
- Read/writes automatically scale up/down with your workloads
- No capacity planning needed
- Pay for what you use, more expensive ($$$)
- Great for unpredictable workloads, steep sudden spikes

### DynamoDB Accelerator (DAX)

Application

- Fully-managed, highly available, seamless in-

memory cache for DynamoDB

- Help solve read congestion by caching

DAX Cluster

- Microseconds latency for cached data
- Doesn’t require application logic modification

Nodes

(compatible with existing DynamoDB APIs)

- 5 minutes TTL for cache (default)

Amazon DynamoDB

Tables


### DynamoDB Accelerator (DAX) vs. ElastiCache

Amazon

ElastiCache

Store Aggregation Result

- Individual objects cache

Application

- Query & Scan cache

Amazon

DynamoDB

DynamoDB Accelerator (DAX)


### DynamoDB – Stream Processing

- Ordered stream of item-level modifications (create/update/delete) in a table
- Use cases:
- React to changes in real-time (welcome email to users)
- Real-time usage analytics
- Insert into derivative tables
- Implement cross-region replication
- Invoke AWS Lambda on changes to your DynamoDB table

DynamoDB Streams Kinesis Data Streams (newer)

- 24 hours retention • 1 year retention
- Limited # of consumers • High # of consumers
- Process using AWS Lambda Triggers, or • Process using AWS Lambda, Kinesis Data

Analytics, Kineis Data Firehose, AWS Glue

DynamoDB Stream Kinesis adapter

Streaming ETL…


### DynamoDB Streams

messaging, notifications

Processing Layer Amazon SNS

DynamoDB

KCL Adapter

filtering, transforming, …

DDB Table

Lambda

create/update/delete

Application DynamoDB

Table analytics Amazon

Streams

Redshift

archiving

Amazon S3

Kinesis Data Kinesis Data

Streams Firehose

indexing Amazon

OpenSearch


### DynamoDB Global Tables

GLOBAL TABLE

Table Table

US-EAST-1 AP-SOUTHEAST-2

two-way

replication

- Make a DynamoDB table accessible with low latency in multiple-regions
- Active-Active replication
- Applications can READ and WRITE to the table in any region
- Must enable DynamoDB Streams as a pre-requisite

### DynamoDB – Time To Live (TTL)

Current Time

Friday, September 10, 2021, 11:56:11 AM

(Epoch timestamp: 1631274971)

Expiration Process

scan &

expire items

- Automatically delete items after an expiry

timestamp SessionData (Table)

- Use cases: reduce stored data by keeping only User_ID Session_ID ExpTime (TTL)

current items, adhere to regulatory 7791a3d6-… 74686572652 1631188571

obligations, web session handling… 873e0634-… 6e6f7468696 1631274971

a80f73a1-… 746f2073656 1631102171

scan &

delete items

Deletion Process


### DynamoDB – Backups for disaster recover y

- Continuous backups using point-in-time recovery (PITR)
- Optionally enabled for the last 35 days
- Point-in-time recovery to any time within the backup window
- The recovery process creates a new table
- On-demand backups
- Full backups for long-term retention, until explicitely deleted
- Doesn’t affect performance or latency
- Can be configured and managed in AWS Backup (enables cross-region copy)
- The recovery process creates a new table

### DynamoDB – Integration with Amazon S3

- Export to S3 (must enable PITR)
- Works for any point of time in the last 35 days
- Doesn’t affect the read capacity of your table
- Perform data analysis on top of DynamoDB export query
- Retain snapshots for auditing
- ETL on top of S3 data before importing back into

DynamoDB S3 Athena

DynamoDB

- Export in DynamoDB JSON or ION format
- Import from S3
- Import CSV, DynamoDB JSON or ION format

import

- Doesn’t consume any write capacity
- Creates a new table

S3 DynamoDB

- Import errors are logged in CloudWatch Logs

(.csv, .json, .ion)


### Example: Building a Ser verless API

REST API PROXY REQUESTS CRUD

Client API Gateway Lambda DynamoDB


### AWS API Gateway

- AWS Lambda + API Gateway: No infrastructure to manage
- Support for the WebSocket Protocol
- Handle API versioning (v1, v2…)
- Handle different environments (dev, test, prod…)
- Handle security (Authentication and Authorization)
- Create API keys, handle request throttling
- Swagger / Open API import to quickly define APIs
- Transform and validate requests and responses
- Generate SDK and API specifications
- Cache API responses

### API Gateway – Integrations High Level

- Lambda Function
- Invoke Lambda function
- Easy way to expose REST API backed by AWS Lambda
- HTTP
- Expose HTTP endpoints in the backend
- Example: internal HTTP API on premise, Application Load Balancer…
- Why? Add rate limiting, caching, user authentications, API keys, etc…
- AWS Service
- Expose any AWS API through the API Gateway
- Example: start an AWS Step Function workflow, post a message to SQS
- Why? Add authentication, deploy publicly, rate control…

### API Gateway – AWS Ser vice Integration

Kinesis Data Streams example

store .json

requests send records files

API Gateway Kinesis Data Kinesis Data Amazon S3

Client

Streams Firehose


### API Gateway - Endpoint Types

- Edge-Optimized (default): For global clients
- Requests are routed through the CloudFront Edge locations (improves latency)
- The API Gateway still lives in only one region
- Regional:
- For clients within the same region
- Could manually combine with CloudFront (more control over the caching

strategies and the distribution)

- Private:
- Can only be accessed from your VPC using an interface VPC endpoint (ENI)
- Use a resource policy to define access

### API Gateway – Security

- User Authentication through
- IAM Roles (useful for internal applications)
- Cognito (identity for external users – example mobile users)
- Custom Authorizer (your own logic)
- Custom Domain Name HTTPS security through integration with AWS

Certificate Manager (ACM)

- If using Edge-Optimized endpoint, then the certificate must be in us-east-1
- If using Regional endpoint, the certificate must be in the API Gateway region
- Must setup CNAME or A-alias record in Route 53

### AWS Step Functions

- Build serverless visual workflow to

orchestrate your Lambda functions

- Features: sequence, parallel, conditions,

timeouts, error handling, …

- Can integrate with EC2, ECS, On-premises

servers, API Gateway, SQS queues, etc…

- Possibility of implementing human approval

feature

- Use cases: order fulfillment, data processing,

web applications, any workflow


### Amazon Cognito

- Give users an identity to interact with our web or mobile application
- Cognito User Pools:
- Sign in functionality for app users
- Integrate with API Gateway & Application Load Balancer
- Cognito Identity Pools (Federated Identity):
- Provide AWS credentials to users so they can access AWS resources directly
- Integrate with Cognito User Pools as an identity provider
- Cognito vs IAM: “hundreds of users”, ”mobile users”, “authenticate with SAML”

### Cognito User Pools (CUP) – User Features

- Create a serverless database of user for your web & mobile apps
- Simple login: Username (or email) / password combination
- Password reset
- Email & Phone Number Verification
- Multi-factor authentication (MFA)
- Federated Identities: users from Facebook, Google, SAML…

### Cognito User Pools (CUP) - Integrations

- CUP integrates with API Gateway and Application Load Balancer

Cognito User Pools

Authenticate Authenticate

Cognito User Pools

Retrieve token

Evaluate Cognito Token

REST API +

Pass Token Application Load Balancer

+ Listeners & Rules

backend Target Group

API Gateway

Backend


### Cognito Identity Pools (Federated Identities)

- Get identities for “users” so they obtain temporary AWS credentials
- Users source can be Cognito User Pools, 3rd party logins, etc…
- Users can then access AWS services directly or through API Gateway
- The IAM policies applied to the credentials are defined in Cognito
- They can be customized based on the user_id for fine grained control
- Default IAM roles for authenticated and guest users

### Cognito Identity Pools – Diagram

Login and Get Token

Social Identity Provider

Web & Mobile Exchange token Cognito Identity Pools

for temporary

Applications

AWS credentials validate

Direct access to AWS

Cognito

User Pools

Private S3 Bucket DynamoDB Table


### Cognito Identity Pools

Row Level Security in DynamoDB


---

## Ser verless Architectures


### Mobile application: MyTodoList

- We want to create a mobile application with the following requirements
- Expose as REST API with HTTPS
- Serverless architecture
- Users should be able to directly interact with their own folder in S3
- Users should authenticate through a managed serverless service
- The users can write and read to-dos, but they mostly read them
- The database should scale, and have some high read throughput

### Mobile app: REST API layer

REST HTTPS invoke query

AWS Lambda Amazon DynamoDB

Amazon API Gateway

Mobile

client

Verify authentication

authenticate

Amazon Cognito


### Mobile app: giving users access to S3

Store/retrieve files

Amazon S3

Permissions

AWS Lambda Amazon DynamoDB

Amazon API Gateway

Mobile

client

authenticate

Amazon Cognito


### Mobile app: high read throughput, static data

Store/retrieve files

Amazon S3

Permissions

REST HTTPS invoke Query / read

AWS Lambda

Amazon API Gateway DAX DynamoDB

Mobile

Caching layer

client

Verify authentication

authenticate

Amazon Cognito


### Mobile app: caching at the API Gateway

Store/retrieve files

Amazon S3

CACHING OF RESPONSES

Permissions

REST HTTPS invoke Query / read

AWS Lambda

Amazon API Gateway DAX DynamoDB

Mobile

Caching layer

client

Verify authentication

authenticate

Amazon Cognito


### In this lecture

- Serverless REST API: HTTPS, API Gateway, Lambda, DynamoDB
- Using Cognito to generate temporary credentials to access S3 bucket

with restricted policy. App users can directly access AWS resources this

way. Pattern can be applied to DynamoDB, Lambda…

- Caching the reads on DynamoDB using DAX
- Caching the REST requests at the API Gateway level
- Security for authentication and authorization with Cognito

### Ser verless hosted website: MyBlog.com

- This website should scale globally
- Blogs are rarely written, but often read
- Some of the website is purely static files, the rest is a dynamic REST API
- Caching must be implement where possible
- Any new users that subscribes should receive a welcome email
- Any photo uploaded to the blog should have a thumbnail generated

### Ser ving static content, globally

Interaction with

edge locations

Amazon S3

Amazon CloudFront

Global distribution

Client


### Ser ving static content, globally, securely

OAC: Origin Access Control

Bucket policy

Only authorize from

Interaction with

CloudFront Distribution

edge locations

Amazon S3

Amazon CloudFront

Global distribution

Client


### Adding a public ser verless REST API

OAC: Origin Access Control

Bucket policy

Only authorize from

Interaction with

CloudFront Distribution

edge locations

Amazon S3

Amazon CloudFront

Global distribution

REST HTTPS invoke Query / read

AWS Lambda

Amazon API Gateway DAX DynamoDB

Client

Caching layer


### Leveraging DynamoDB Global Tables

OAC: Origin Access Control

Bucket policy

Only authorize from

Interaction with

CloudFront Distribution

edge locations

Amazon S3

Amazon CloudFront

Global distribution

REST HTTPS invoke Query / read

AWS Lambda

Amazon API Gateway DAX DynamoDB

Client

Caching layer Global Tables


### User Welcome email flow

OAC: Origin Access Control

Bucket policy

Only authorize from

Interaction with

CloudFront Distribution

edge locations

Amazon S3

Amazon CloudFront

Global distribution

REST HTTPS invoke Query / read

AWS Lambda

Amazon API Gateway DAX DynamoDB

Client

Caching layer

Stream changes

IAM Role

SDK to send email

Invoke lambda

Amazon Simple AWS Lambda

DynamoDB

Email Service (SES)

Stream


### Thumbnail Generation flow

OAC: Origin Access Control

Bucket policy

Only authorize from

Interaction with

CloudFront Distribution

edge locations

Amazon S3

Amazon CloudFront

Global distribution

REST HTTPS invoke Query / read

AWS Lambda

Amazon API Gateway DAX DynamoDB

Client

Caching layer

optional

Upload photos

Transfer acceleration OAC trigger thumbnail

Amazon CloudFront Amazon S3 AWS Lambda Amazon S3

Global distribution


### AWS Hosted Website Summary

- We’ve seen static content being distributed using CloudFront with S3
- The REST API was serverless, didn’t need Cognito because public
- We leveraged a Global DynamoDB table to serve the data globally
- (we could have used Aurora Global Database)
- We enabled DynamoDB streams to trigger a Lambda function
- The lambda function had an IAM role which could use SES
- SES (Simple Email Service) was used to send emails in a serverless way
- S3 can trigger SQS / SNS / Lambda to notify of events

### Micro Ser vices architecture

- We want to switch to a micro service architecture
- Many services interact with each other directly using a REST API
- Each architecture for each micro service may vary in form and shape
- We want a micro-service architecture so we can have a leaner

development lifecycle for each service


### Micro Ser vices Environment

service1.example.com

DNS Query

Amazon Route 53

Elastic Load Balancing ECS DynamoDB

service2.example.com

HTTPS

Amazon API Gateway AWS Lambda ElastiCache

Users

service3.example.com

Elastic Load Balancing Amazon EC2

Amazon RDS

Auto Scaling


### Discussions on Micro Ser vices

- You are free to design each micro-service the way you want
- Synchronous patterns: API Gateway, Load Balancers
- Asynchronous patterns: SQS, Kinesis, SNS, Lambda triggers (S3)
- Challenges with micro-services:
- repeated overhead for creating each new microservice,
- issues with optimizing server density/utilization
- complexity of running multiple versions of multiple microservices simultaneously
- proliferation of client-side code requirements to integrate with many separate services.
- Some of the challenges are solved by Serverless patterns:
- API Gateway, Lambda scale automatically and you pay per usage
- You can easily clone API, reproduce environments
- Generated client SDK through Swagger integration for the API Gateway

### Software updates offloading

- We have an application running on EC2, that distributes software

updates once in a while

- When a new software update is out, we get a lot of request and the

content is distributed in mass over the network. It’s very costly

- We don’t want to change our application, but want to optimize our cost

and CPU, how can we do it?


### Our application current state

Auto Scaling group

Availability zone 1

Availability zone 1 to 3

Availability zone 2

Amazon Elastic

File System

Availability zone 3


### Easy way to fix things!

Auto Scaling group

Availability zone 1

Availability zone 1 to 3

Availability zone 2

Amazon Elastic

Amazon CloudFront File System

Availability zone 3


### Why CloudFront?

- No changes to architecture
- Will cache software update files at the edge
- Software update files are not dynamic, they’re static (never changing)
- Our EC2 instances aren’t serverless
- But CloudFront is, and will scale for us
- Our ASG will not scale as much, and we’ll save tremendously in EC2
- We’ll also save in availability, network bandwidth cost, etc
- Easy way to make an existing application more scalable and cheaper!

---

## Databases in AWS


### Choosing the Right Database

- We have a lot of managed databases on AWS to choose from
- Questions to choose the right database based on your architecture:
- Read-heavy, write-heavy, or balanced workload? Throughput needs? Will it

change, does it need to scale or fluctuate during the day?

- How much data to store and for how long? Will it grow? Average object size?

How are they accessed?

- Data durability? Source of truth for the data ?
- Latency requirements? Concurrent users?
- Data model? How will you query the data? Joins? Structured? Semi-Structured?
- Strong schema? More flexibility? Reporting? Search? RDBMS / NoSQL?
- License costs? Switch to Cloud Native DB such as Aurora?

### Database Types

- RDBMS (= SQL / OLTP): RDS, Aurora – great for joins
- NoSQL database – no joins, no SQL : DynamoDB (~JSON), ElastiCache (key /

value pairs), Neptune (graphs), DocumentDB (for MongoDB), Keyspaces (for

Apache Cassandra)

- Object Store: S3 (for big objects) / Glacier (for backups / archives)
- Data Warehouse (= SQL Analytics / BI): Redshift (OLAP), Athena, EMR
- Search: OpenSearch (JSON) – free text, unstructured searches
- Graphs: Amazon Neptune – displays relationships between data
- Ledger: Amazon Quantum Ledger Database
- Time series: Amazon Timestream
- Note: some databases are being discussed in the Data & Analytics section

### Amazon RDS – Summary

- Managed PostgreSQL / MySQL / Oracle / SQL Server / DB2 / MariaDB / Custom
- Provisioned RDS Instance Size and EBS Volume Type & Size
- Auto-scaling capability for Storage
- Support for Read Replicas and Multi AZ
- Security through IAM, Security Groups, KMS , SSL in transit
- Automated Backup with Point in time restore feature (up to 35 days)
- Manual DB Snapshot for longer-term recovery
- Managed and Scheduled maintenance (with downtime)
- Support for IAM Authentication, integration with Secrets Manager
- RDS Custom for access to and customize the underlying instance (Oracle & SQL Server)
- Use case: Store relational datasets (RDBMS / OLTP), perform SQL queries, transactions

### Amazon Aurora – Summary

- Compatible API for PostgreSQL / MySQL, separation of storage and compute
- Storage: data is stored in 6 replicas, across 3 AZ – highly available, self-healing, auto-scaling
- Compute: Cluster of DB Instance across multiple AZ, auto-scaling of Read Replicas
- Cluster: Custom endpoints for writer and reader DB instances
- Same security / monitoring / maintenance features as RDS
- Know the backup & restore options for Aurora
- Aurora Serverless – for unpredictable / intermittent workloads, no capacity planning
- Aurora Global: up to 16 DB Read Instances in each region, < 1 second storage replication
- Aurora Machine Learning: perform ML using SageMaker & Comprehend on Aurora
- Aurora Database Cloning: new cluster from existing one, faster than restoring a snapshot
- Use case: same as RDS, but with less maintenance / more flexibility / more performance / more features

### Amazon ElastiCache – Summary

- Managed Redis / Memcached (similar offering as RDS, but for caches)
- In-memory data store, sub-millisecond latency
- Select an ElastiCache instance type (e.g., cache.m6g.large)
- Support for Clustering (Redis) and Multi AZ, Read Replicas (sharding)
- Security through IAM, Security Groups, KMS, Redis Auth
- Backup / Snapshot / Point in time restore feature
- Managed and Scheduled maintenance
- Requires some application code changes to be leveraged
- Use Case: Key/Value store, Frequent reads, less writes, cache results for DB

queries, store session data for websites, cannot use SQL.


### Amazon DynamoDB – Summary

- AWS proprietary technology, managed serverless NoSQL database, millisecond latency
- Capacity modes: provisioned capacity with optional auto-scaling or on-demand capacity
- Can replace ElastiCache as a key/value store (storing session data for example, using TTL feature)
- Highly Available, Multi AZ by default, Read and Writes are decoupled, transaction capability
- DAX cluster for read cache, microsecond read latency
- Security, authentication and authorization is done through IAM
- Event Processing: DynamoDB Streams to integrate with AWS Lambda, or Kinesis Data Streams
- Global Table feature: active-active setup
- Automated backups up to 35 days with PITR (restore to new table), or on-demand backups
- Export to S3 without using RCU within the PITR window, import from S3 without using WCU
- Great to rapidly evolve schemas
- Use Case: Serverless applications development (small documents 100s KB), distributed serverless

cache


### Amazon S3 – Summary

- S3 is a… key / value store for objects
- Great for bigger objects, not so great for many small objects
- Serverless, scales infinitely, max object size is 5 TB, versioning capability
- Tiers: S3 Standard, S3 Infrequent Access, S3 Intelligent, S3 Glacier + lifecycle policy
- Features: Versioning, Encryption, Replication, MFA-Delete, Access Logs…
- Security: IAM, Bucket Policies, ACL, Access Points, Object Lambda, CORS, Object/Vault Lock
- Encryption: SSE-S3, SSE-KMS, SSE-C, client-side, TLS in transit, default encryption
- Batch operations on objects using S3 Batch, listing files using S3 Inventory
- Performance: Multi-part upload, S3 Transfer Acceleration, S3 Select
- Automation: S3 Event Notifications (SNS, SQS, Lambda, EventBridge)
- Use Cases: static files, key value store for big files, website hosting

### DocumentDB

- Aurora is an “AWS-implementation” of PostgreSQL / MySQL …
- DocumentDB is the same for MongoDB (which is a NoSQL database)
- MongoDB is used to store, query, and index JSON data
- Similar “deployment concepts” as Aurora
- Fully Managed, highly available with replication across 3 AZ
- DocumentDB storage automatically grows in increments of 10GB
- Automatically scales to workloads with millions of requests per seconds

### Amazon Neptune

- Fully managed graph database
- A popular graph dataset would be a social network
- Users have friends
- Posts have comments
- Comments have likes from users
- Users share and like posts…
- Highly available across 3 AZ, with up to 15 read replicas
- Build and run applications working with highly connected

datasets – optimized for these complex and hard queries

- Can store up to billions of relations and query the graph with

milliseconds latency

- Highly available with replications across multiple AZs
- Great for knowledge graphs (Wikipedia), fraud detection,

recommendation engines, social networking


### Amazon Neptune – Streams

- Real-time ordered sequence of every change to

Neptune Cluster

your graph data

writes

- Changes are available immediately after writing
- No duplicates, strict order

Neptune

- Streams data is accessible in an HTTP REST API Streams

Streams API

- Use cases:

HTTP Get Request

- Send notifications when certain changes are made

Streams reader application

- Maintain your graph data synchronized in another …

data store (e.g., S3, OpenSearch, ElastiCache)

- Replicate data across regions in Neptune

S3 OpenSearch ElastiCache


### Amazon Keyspaces (for Apache Cassandra)

- Apache Cassandra is an open-source NoSQL distributed database
- A managed Apache Cassandra-compatible database service
- Serverless, Scalable, highly available, fully managed by AWS
- Automatically scale tables up/down based on the application’s traffic
- Tables are replicated 3 times across multiple AZ
- Using the Cassandra Query Language (CQL)
- Single-digit millisecond latency at any scale, 1000s of requests per second
- Capacity: On-demand mode or provisioned mode with auto-scaling
- Encryption, backup, Point-In-Time Recovery (PITR) up to 35 days
- Use cases: store IoT devices info, time-series data, …

### Amazon Timestream

- Fully managed, fast, scalable, serverless time series database
- Automatically scales up/down to adjust capacity
- Store and analyze trillions of events per day
- 1000s times faster & 1/10th the cost of relational databases
- Scheduled queries, multi-measure records, SQL compatibility
- Data storage tiering: recent data kept in memory and

historical data kept in a cost-optimized storage

- Built-in time series analytics functions (helps you identify

patterns in your data in near real-time)

- Encryption in transit and at rest
- Use cases: IoT apps, operational applications, real-time

analytics, …


### Amazon Timestream – Architecture

AWS IoT Amazon

QuickSight

Lambda

Kinesis Data

Amazon

Streams

SageMaker

Prometheus

Amazon

Timestream

Kinesis Data

Streams

Any JDBC connection

Amazon MSK

Kinesis Data Analytics

For Apache Flink


---

## Data & Analytics


### Amazon Athena

- Serverless query service to analyze data stored in Amazon S3
- Uses standard SQL language to query the files (built on Presto)

load data

- Supports CSV, JSON, ORC, Avro, and Parquet
- Pricing: $5.00 per TB of data scanned

S3 Bucket

- Commonly used with Amazon Quicksight for

reporting/dashboards

Query & Analyze

Amazon

- Use cases: Business intelligence / analytics / reporting, analyze &

Athena

query VPC Flow Logs, ELB Logs, CloudTrail trails, etc...

- Exam Tip: analyze data in S3 using serverless SQL, use Athena Reporting & Dashboards

Amazon

QuickSight


### Amazon Athena – Performance Improvement

- Use columnar data for cost-savings (less scan)
- Apache Parquet or ORC is recommended
- Huge performance improvement
- Use Glue to convert your data to Parquet or ORC
- Compress data for smaller retrievals (bzip2, gzip, lz4, snappy, zlip, zstd…)
- Partition datasets in S3 for easy querying on virtual columns
- s3://yourBucket/pathToTable

/<PARTITION_COLUMN_NAME>=<VALUE>

/<PARTITION_COLUMN_NAME>=<VALUE>

/<PARTITION_COLUMN_NAME>=<VALUE>

/etc…

- Example: s3://athena-examples/flight/parquet/year=1991/month=1/day=1/
- Use larger files (> 128 MB) to minimize overhead

### Amazon Athena – Federated Quer y

Amazon

- Allows you to run SQL queries across

Athena

data stored in relational, non-relational,

S3 Bucket

object, and custom data sources (AWS

or on-premises) Lambda

(Data Source

ElastiCache Connector)

- Uses Data Source Connectors that run

on AWS Lambda to run Federated

Queries (e.g., CloudWatch Logs,

DocumentDB

HBase in EMR

DynamoDB, RDS, …)

- Store the results back in Amazon S3

DynamoDB

Database

(On-Premises)

Redshift

Aurora SQL Server MySQL


### Redshift Over view

- Redshift is based on PostgreSQL, but it’s not used for OLTP
- It’s OLAP – online analytical processing (analytics and data warehousing)
- 10x better performance than other data warehouses, scale to PBs of data
- Columnar storage of data (instead of row based) & parallel query engine
- Two modes: Provisioned cluster or Serverless cluster
- Has a SQL interface for performing the queries
- BI tools such as Amazon Quicksight or Tableau integrate with it
- vs Athena: faster queries / joins / aggregations thanks to indexes

### Redshift Cluster

- Leader node: for query

SELECT COUNT (*), …

Query FROM MY_TABLE planning, results

GROUP BY …

aggregation

JDBC/ODBC

- Compute node: for

performing the queries,

Amazon Redshift Cluster

send results to leader

Leader Node

- Provisioned mode:
- Choose instance types

in advance

Compute Nodes

- Can reserve instances

for cost savings


### Redshift – Snapshots & DR

Region

- Redshift has “Multi-AZ” mode for some clusters (us-east-1)
- Snapshots are point-in-time backups of a cluster,

stored internally in S3 Take Snapshot Cluster

Snapshot

- Snapshots are incremental (only what has

changed is saved)

Redshift Cluster

- You can restore a snapshot into a new cluster

(Original)

- Automated: every 8 hours, every 5 GB, or on a Automated

schedule. Set retention between 1 to 35 days

/ Manual

- Manual: snapshot is retained until you delete it Copy

Region

(eu-west-1)

- You can configure Amazon Redshift to

automatically copy snapshots (automated or

Restore Copied

manual) of a cluster to another AWS Region

Snapshot

Redshift Cluster

(New)


### Loading data into Redshift:

Large inser ts are MUCH better

Amazon Kinesis S3 using COPY command EC2 Instance

Data Firehose JDBC driver

Internet

Without Enhanced VPC Routing

Through VPC

With Enhanced VPC Routing

Amazon Kinesis Amazon Redshift

S3 Bucket Amazon Redshift EC2 Instance Amazon Redshift

Data Firehose Cluster

(mybucket) Cluster Cluster

(through S3 copy) Better to write

Data in batches

copy customer

from 's3://mybucket/mydata’

iam_role 'arn:aws:iam::0123456789012:role/MyRedshiftRole';


### Redshift Spectrum

SELECT COUNT (*), …

Query FROM S3.EXT_TABLE

GROUP BY …

JDBC/ODBC

- Query data that is already in

Amazon Redshift Cluster

S3 without loading it

Leader Node

- Must have a Redshift cluster

available to start the query

- The query is then submitted Compute Nodes

to thousands of Redshift

Spectrum nodes

Redshift Spectrum

1 2 …. N

Amazon S3


|  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |  |  |  |


### Amazon OpenSearch Ser vice

- Amazon OpenSearch is successor to Amazon ElasticSearch
- In DynamoDB, queries only exist by primary key or indexes…
- With OpenSearch, you can search any field, even partially matches
- It’s common to use OpenSearch as a complement to another database
- Two modes: managed cluster or serverless cluster
- Does not natively support SQL (can be enabled via a plugin)
- Ingestion from Kinesis Data Firehose, AWS IoT, and CloudWatch Logs
- Security through Cognito & IAM, KMS encryption, TLS
- Comes with OpenSearch Dashboards (visualization)

### OpenSearch patterns

DynamoDB

CRUD

DynamoDB Table DynamoDB Stream Lambda Function Amazon OpenSearch

API to search items

API to retrieve items


### OpenSearch patterns

CloudWatch Logs

Real time

CloudWatch Logs Subscription Filter Lambda Function Amazon OpenSearch

(managed by AWS)

Near Real Time

CloudWatch Logs Subscription Filter Kinesis Data Firehose Amazon OpenSearch


### OpenSearch patterns

Kinesis Data Streams & Kinesis Data Firehose

Kinesis Data

Kinesis Data

Streams

Streams

Lambda

Function

Kinesis Data

data Lambda

Firehose

transformation

Function

(near real time)

(real time)

Amazon

Amazon

OpenSearch

OpenSearch


### Amazon EMR

- EMR stands for “Elastic MapReduce”
- EMR helps creating Hadoop clusters (Big Data) to analyze and process

vast amount of data

- The clusters can be made of hundreds of EC2 instances
- EMR comes bundled with Apache Spark, HBase, Presto, Flink…
- EMR takes care of all the provisioning and configuration
- Auto-scaling and integrated with Spot instances
- Use cases: data processing, machine learning, web indexing, big data…

### Amazon EMR – Node types & purchasing

- Master Node: Manage the cluster, coordinate, manage health – long running
- Core Node: Run tasks and store data – long running
- Task Node (optional): Just to run tasks – usually Spot
- Purchasing options:
- On-demand: reliable, predictable, won’t be terminated
- Reserved (min 1 year): cost savings (EMR will automatically use if available)
- Spot Instances: cheaper, can be terminated, less reliable
- Can have long-running cluster, or transient (temporary) cluster

### Amazon QuickSight

- Serverless machine learning-powered business intelligence service to create

interactive dashboards

- Fast, automatically scalable, embeddable, with per-session pricing
- Use cases:
- Business analytics
- Building visualizations
- Perform ad-hoc analysis
- Get business insights using data
- Integrated with RDS, Aurora,

Athena, Redshift, S3…

- In-memory computation using SPICE

engine if data is imported into QuickSight

- Enterprise edition: Possibility to setup

Column-Level security (CLS)

https://aws.amazon.com/quicksight/


### QuickSight Integrations

Data Sources (AWS Services)

On-Premises

QuickSight

Databases (JDBC)

RDS Aurora Redshift

Data Sources (Imports)

Athena S3 OpenSearch

Data Sources (SaaS)

ELF & CLF

Timestream

(Log Format)


### QuickSight – Dashboard & Analysis

- Define Users (standard versions) and Groups (enterprise version)
- These users & groups only exist within QuickSight, not IAM !!
- A dashboard…
- is a read-only snapshot of an analysis that you can share
- preserves the configuration of the analysis (filtering, parameters, controls, sort)
- You can share the analysis or the dashboard with Users or Groups
- To share a dashboard, you must first publish it
- Users who see the dashboard can also see the underlying data

### AWS Glue

- Managed extract, transform, and load (ETL) service
- Useful to prepare and transform data for analytics
- Fully serverless service

Glue ETL

S3 Bucket

Extract Load

Amazon RDS Redshift

Transform

Data Warehouse


### AWS Glue – Conver t data into Parquet format

Glue ETL

Import CSV

S3 Put Parquet Analyze

Amazon

Athena

Input Output

S3 Bucket S3 Bucket

Trigger

Glue ETL Job

Event notifications

On S3 PUT

Lambda Function

(EventBridge works as an alternative)


### Glue Data Catalog: catalog of datasets

Glue Jobs (ETL)

Amazon Athena

Amazon S3

Data discovery

Writes Metadata

AWS Glue

Amazon RDS

Data Catalog

Amazon

AWS Glue

Redshift

Data Crawler Database Database

Spectrum

Amazon DynamoDB

JDBC Tables Tables

(Metadata) (Metadata)

Amazon EMR


### Glue – things to know at a high-level

- Glue Job Bookmarks: prevent re-processing old data
- Glue DataBrew: clean and normalize data using pre-built transformation
- Glue Studio: new GUI to create, run and monitor ETL jobs in Glue
- Glue Streaming ETL (built on Apache Spark Structured Streaming):

compatible with Kinesis Data Streaming, Kafka, MSK (managed Kafka)


### AWS Lake Formation

- Data lake = central place to have all your data for analytics purposes
- Fully managed service that makes it easy to setup a data lake in days
- Discover, cleanse, transform, and ingest data into your Data Lake
- It automates many complex manual steps (collecting, cleansing, moving,

cataloging data, …) and de-duplicate (using ML Transforms)

- Combine structured and unstructured data in the data lake
- Out-of-the-box source blueprints: S3, RDS, Relational & NoSQL DB…
- Fine-grained Access Control for your applications (row and column-level)
- Built on top of AWS Glue

### AWS Lake Formation

Data Sources

Athena

Source Crawlers

Amazon S3 ETL and Data Prep.

ingest

Data Catalog Redshift

Security Settings Users

RDS Aurora

Access Control

AWS Lake Formation

On-Premises

Database (SQL & NoSQL)

Data Lake

(stored in S3)


### AWS Lake Formation

Centralized Permissions Example

Data Sources

ingest

Access Control

Amazon S3

Column-level security

Athena Users

AWS Lake Formation Quicksight

RDS Aurora

Data Lake

(stored in S3)


### Amazon Managed Ser vice for Apache Flink

- Previously named: Kinesis Data Analytics for Apache Flink
- Flink (Java, Scala or SQL) is a framework for processing data streams

Kinesis Data

Streams

Amazon MSK

Amazon Managed Service

(Apache Kafka)

for Apache Flink

- Run any Apache Flink application on a managed cluster on AWS
- Provisioned compute resources, parallel computation, automatic scaling
- Application backups (implemented as checkpoints and snapshots)
- Use any Apache Flink programming features to transform data
- Important: Flink does not read from Amazon Data Firehose

### Amazon Managed Streaming for Apache

Kafka (Amazon MSK)

- Alternative to Amazon Kinesis
- Fully managed Apache Kafka on AWS
- Allow you to create, update, delete clusters
- MSK creates & manages Kafka brokers nodes & Zookeeper nodes for you
- Deploy the MSK cluster in your VPC, multi-AZ (up to 3 for HA)
- Automatic recovery from common Apache Kafka failures
- Data is stored on EBS volumes for as long as you want
- MSK Serverless
- Run Apache Kafka on MSK without managing the capacity
- MSK automatically provisions resources and scales compute & storage

### Apache Kafka at a high level

MSK Cluster

Kinesis

n S3

a Broker 2

SageMaker

Producers Write to topic Poll from topic Consumers

(your code) (your code)

Broker 1

RDS lic Kinesis

Etc…

Broker 3

Etc…


### Kinesis Data Streams vs. Amazon MSK

Kinesis Data Streams Amazon MSK

- 1 MB message size limit • 1MB default, configure for higher (ex: 10MB)
- Data Streams with Shards • Kafka Topics with Partitions
- Shard Splitting & Merging • Can only add partitions to a topic
- TLS In-flight encryption • PLAINTEXT or TLS In-flight Encryption
- KMS at-rest encryption • KMS at-rest encryption

### Amazon MSK Consumers

Kinesis Data Analytics

for Apache Flink

AWS Glue

Streaming ETL Jobs

Powered by Apache Spark Streaming

Lambda

Amazon MSK

Applications Running on

Amazon EC2 ECS EKS


### Big Data Ingestion Pipeline

- We want the ingestion pipeline to be fully serverless
- We want to collect data in real time
- We want to transform the data
- We want to query the transformed data using SQL
- The reports created using the queries should be in S3
- We want to load that data into a warehouse and create dashboards

### Big Data Ingestion Pipeline

Pull data

IoT Devices

Real-time Reporting

Ingestion

Every 1 minute Bucket

Bucket

trigger

Amazon Kinesis Data Amazon Kinesis Data Amazon Simple Storage Amazon Simple Queue AWS Lambda Amazon Athena Amazon Simple Storage

Streams Firehose Service (S3) Service Service (S3)

(optional)

AWS Lambda

Amazon QuickSight

Amazon Redshift

Serverless


### Big Data Ingestion Pipeline discussion

- IoT Core allows you to harvest data from IoT devices
- Kinesis is great for real-time data collection
- Firehose helps with data delivery to S3 in near real-time (1 minute)
- Lambda can help Firehose with data transformations
- Amazon S3 can trigger notifications to SQS
- Lambda can subscribe to SQS (we could have connecter S3 to Lambda)
- Athena is a serverless SQL service and results are stored in S3
- The reporting bucket contains analyzed data and can be used by

reporting tool such as AWS QuickSight, Redshift, etc…


---

## Machine Learning


### Amazon Rekognition

- Find objects, people, text, scenes in images and videos using ML
- Facial analysis and facial search to do user verification, people counting
- Create a database of “familiar faces” or compare against celebrities
- Use cases:
- Labeling
- Content Moderation
- Text Detection
- Face Detection and Analysis (gender, age range, emotions…)
- Face Search and Verification
- Celebrity Recognition
- Pathing (ex: for sports game analysis)

### Amazon Rekognition – Content Moderation

- Detect content that is inappropriate, unwanted,

Image

or offensive (image and videos)

- Used in social media, broadcast media,

advertising, and e-commerce situations to create

Amazon

a safer user experience

Rekognition

- Set a Minimum Confidence Threshold for items

that will be flagged

- Flag sensitive content for manual review in

Confidence Level

Amazon Augmented AI (A2I)

and Threshold

- Help comply with regulations

Optional Manual

review in A2I


### Amazon Transcribe

- Automatically convert speech to text
- Uses a deep learning process called automatic speech recognition (ASR) to convert

speech to text quickly and accurately

- Automatically remove Personally Identifiable Information (PII) using Redaction
- Supports Automatic Language Identification for multi-lingual audio
- Use cases:
- transcribe customer service calls
- automate closed captioning and subtitling
- generate metadata for media assets to create a fully searchable archive

”Hello my name is Stéphane.

I hope you’re enjoying the course!


### Amazon Polly

- Turn text into lifelike speech using deep learning
- Allowing you to create applications that talk

Hi! My name is Stéphane

and this is a demo of Amazon Polly


### Amazon Polly – Lexicon & SSML

- Customize the pronunciation of words with Pronunciation lexicons
- Stylized words: St3ph4ne => “Stephane”
- Acronyms: AWS => “Amazon Web Services”
- Upload the lexicons and use them in the SynthesizeSpeech operation
- Generate speech from plain text or from documents marked up with Speech

Synthesis Markup Language (SSML) – enables more customization

- emphasizing specific words or phrases
- using phonetic pronunciation
- including breathing sounds, whispering
- using the Newscaster speaking style

### Amazon Translate

- Natural and accurate language translation
- Amazon Translate allows you to localize content - such as websites and

applications - for international users, and to easily translate large

volumes of text efficiently.


### Amazon Lex & Connect

- Amazon Lex: (same technology that powers Alexa)
- Automatic Speech Recognition (ASR) to convert speech to text
- Natural Language Understanding to recognize the intent of text, callers
- Helps build chatbots, call center bots
- Amazon Connect:
- Receive calls, create contact flows, cloud-based virtual contact center
- Can integrate with other CRM systems or AWS
- No upfront payments, 80% cheaper than traditional contact center solutions

Phone Call

call stream invoke schedule

Schedule an

Appointment

Connect Lex Lambda

Intent recognized


### Amazon Comprehend

- For Natural Language Processing – NLP
- Fully managed and serverless service
- Uses machine learning to find insights and relationships in text
- Language of the text
- Extracts key phrases, places, people, brands, or events
- Understands how positive or negative the text is
- Analyzes text using tokenization and parts of speech
- Automatically organizes a collection of text files by topic
- Sample use cases:
- analyze customer interactions (emails) to find what leads to a positive or negative experience
- Create and groups articles by topics that Comprehend will uncover

### Amazon Comprehend Medical

- Amazon Comprehend Medical detects and returns useful information in

unstructured clinical text:

- Physician’s notes
- Discharge summaries
- Test results
- Case notes
- Uses NLP to detect Protected Health Information (PHI) – DetectPHI API
- Store your documents in Amazon S3, analyze real-time data with Kinesis

Data Firehose, or use Amazon Transcribe to transcribe patient narratives

into text that can be analyzed by Amazon Comprehend Medical.


### Amazon SageMaker AI

- Fully managed service for developers / data scientists to build ML models
- Typically, difficult to do all the processes in one place + provision servers
- Machine learning process (simplified): predicting your exam score

label build

Train and Tune

ML model

Historical Data:

score

# years of experience in IT

Apply model

# years of experience with AWS

Time spent on the course

PASS WITH 906

New data Prediction


### Amazon Kendra

- Fully managed document search service powered by Machine Learning
- Extract answers from within a document (text, pdf, HTML, PowerPoint, MS Word, FAQs…)
- Natural language search capabilities
- Learn from user interactions/feedback to promote preferred results (Incremental Learning)
- Ability to manually fine-tune search results (importance of data, freshness, custom, …)

Data Sources

Where is the IT support desk?

indexing

Amazon S3 Amazon RDS Google Drive MS SharePoint

1st floor

Knowledge Index

User

3rd party, (powered by ML)

APNs,

MS OneDrive Amazon Kendra

Custom


### Amazon Personalize

- Fully managed ML-service to build apps with real-time personalized recommendations
- Example: personalized product recommendations/re-ranking, customized direct marketing
- Example: User bought gardening tools, provide recommendations on the next one to buy
- Same technology used by Amazon.com
- Integrates into existing websites, applications, SMS, email marketing systems, …
- Implement in days, not months (you don’t need to build, train, and deploy ML solutions)
- Use cases: retail stores, media and entertainment…

Websites & Apps

read data from S3

Amazon S3

Mobile Apps

Customized personalized API

real-time data integration

Amazon Personalize API

Amazon Personalize

Emails


### Amazon Textract

- Automatically extracts text, handwriting, and data from any scanned

documents using AI and ML

“Document ID”: “123456789-005”,

“Name”: “”,

analyze result

“SEX”: “F”,

“DOB”: “23.05.1997”,

Amazon Textract

- Extract data from forms and tables
- Read and process any type of document (PDFs, images, …)
- Use cases:
- Financial Services (e.g., invoices, financial reports)
- Healthcare (e.g., medical records, insurance claims)
- Public Sector (e.g., tax forms, ID documents, passports)

### AWS Machine Learning - Summary

- Rekognition: face detection, labeling, celebrity recognition
- Transcribe: audio to text (ex: subtitles)
- Polly: text to audio
- Translate: translations
- Lex: build conversational bots – chatbots
- Connect: cloud contact center
- Comprehend: natural language processing
- SageMaker: machine learning for every developer and data scientist
- Kendra: ML-powered search engine
- Personalize: real-time personalized recommendations
- Textract: detect text and data in documents

### AWS Monitoring, Audit and

Performance

CloudWatch, CloudTrail & AWS Config


### Amazon CloudWatch Metrics

- CloudWatch provides metrics for every services in AWS
- Metric is a variable to monitor (CPUUtilization, NetworkIn…)
- Metrics belong to namespaces
- Dimension is an attribute of a metric (instance id, environment, etc…).
- Up to 30 dimensions per metric
- Metrics have timestamps
- Can create CloudWatch dashboards of metrics
- Can create CloudWatch Custom Metrics (for the RAM for example)

### CloudWatch Metric Streams

CloudWatch Metrics

- Continually stream CloudWatch

metrics to a destination of your choice,

with near-real-time delivery and low Stream near-real-time

latency.

- Amazon Kinesis Data Firehose (and then

Kinesis Data Firehose

its destinations)

- 3rd party service provider: Datadog,

Dynatrace, New Relic, Splunk, Sumo

Logic…

- Option to filter metrics to only stream

Amazon S3 Amazon Amazon

a subset of them

Redshift OpenSearch

Athena


### CloudWatch Logs

- Log groups: arbitrary name, usually representing an application
- Log stream: instances within application / log files / containers
- Can define log expiration policies (never expire, 1 day to 10 years…)
- CloudWatch Logs can send logs to:
- Amazon S3 (exports)
- Kinesis Data Streams
- Kinesis Data Firehose
- AWS Lambda
- OpenSearch
- Logs are encrypted by default
- Can setup KMS-based encryption with your own keys

### CloudWatch Logs - Sources

- SDK, CloudWatch Logs Agent, CloudWatch Unified Agent
- Elastic Beanstalk: collection of logs from application
- ECS: collection from containers
- AWS Lambda: collection from function logs
- VPC Flow Logs: VPC specific logs
- API Gateway
- CloudTrail based on filter
- Route53: Log DNS queries

### CloudWatch Logs Insights

https://mng.workshop.aws/operations-2022/detect/cwlogs.html


### CloudWatch Logs Insights

- Search and analyze log data stored in CloudWatch Logs
- Example: find a specific IP inside a log, count occurrences of

“ERROR” in your logs…

- Provides a purpose-built query language
- Automatically discovers fields from AWS services and JSON log

events

- Fetch desired event fields, filter based on conditions, calculate

aggregate statistics, sort events, limit number of events…

- Can save queries and add them to CloudWatch Dashboards
- Can query multiple Log Groups in different AWS accounts
- It’s a query engine, not a real-time engine

### CloudWatch Logs – S3 Expor t

- Log data can take up to 12 hours to

become available for export

- The API call is CreateExportTask

CloudWatch Logs Amazon S3

- Not near-real time or real-time… use

Logs Subscriptions instead


### CloudWatch Logs Subscriptions

- Get a real-time log events from CloudWatch Logs for processing and analysis
- Send to Kinesis Data Streams, Kinesis Data Firehose, or Lambda
- Subscription Filter – filter which logs are events delivered to your destination

real-time OpenSearch

Service

Lambda

near

logs real-time

CloudWatch Logs Kinesis Data Firehose

Subscription Filter

KDF KDA EC2 Lambda

Kinesis Data Streams


### CloudWatch Logs Aggregation

Multi-Account & Multi Region

ACCOUNT A

REGION 1

CloudWatch Logs Subscription Filter

ACCOUNT B

Near

REGION 2

Real Time

CloudWatch Logs Subscription Filter Kinesis Data Streams Kinesis Data Firehose Amazon S3

ACCOUNT B

REGION 3

CloudWatch Logs Subscription Filter


### CloudWatch Logs Subscriptions

- Cross-Account Subscription – send log events to resources in a different AWS

account (KDS, KDF)

IAM Role

(Cross-Account)

Account – Sender Account – Recipient

(111111111111) (999999999999)

logs logs

CloudWatch Subscription Subscription Kinesis Data Streams

Logs Filter Destination (RecipientStream)

Destination

Destination

Access Policy

Access Policy

Can be assumed

IAM Role

allow PutRecord


### CloudWatch Logs for EC2

- By default, no logs from your EC2

machine will go to CloudWatch

CloudWatch Logs

- You need to run a CloudWatch

agent on EC2 to push the log files

you want

- Make sure IAM permissions are

correct

CloudWatch CloudWatch

Logs Agent Logs Agent

- The CloudWatch log agent can be

setup on-premises too On Premise

EC2 Instance Server


### CloudWatch Logs Agent & Unified Agent

- For virtual servers (EC2 instances, on-premises servers…)
- CloudWatch Logs Agent
- Old version of the agent
- Can only send to CloudWatch Logs
- CloudWatch Unified Agent
- Collect additional system-level metrics such as RAM, processes, etc…
- Collect logs to send to CloudWatch Logs
- Centralized configuration using SSM Parameter Store

### CloudWatch Unified Agent – Metrics

- Collected directly on your Linux server / EC2 instance
- CPU (active, guest, idle, system, user, steal)
- Disk metrics (free, used, total), Disk IO (writes, reads, bytes, iops)
- RAM (free, inactive, used, total, cached)
- Netstat (number of TCP and UDP connections, net packets, bytes)
- Processes (total, dead, bloqued, idle, running, sleep)
- Swap Space (free, used, used %)
- Reminder: out-of-the box metrics for EC2 – disk, CPU, network (high level)

### CloudWatch Alarms

- Alarms are used to trigger notifications for any metric
- Various options (sampling, %, max, min, etc…)
- Alarm States:
- OK
- INSUFFICIENT_DATA
- ALARM
- Period:
- Length of time in seconds to evaluate the metric
- High resolution custom metrics: 10 sec, 30 sec or multiples of 60 sec

### CloudWatch Alarm Targets

- Stop, Terminate, Reboot, or Recover an EC2 Instance
- Trigger Auto Scaling Action
- Send notification to SNS (from which you can do pretty much anything)

Amazon EC2 EC2 Auto Scaling Amazon SNS


### CloudWatch Alarms – Composite Alarms

- CloudWatch Alarms are on a single metric
- Composite Alarms are monitoring the states of multiple other alarms
- AND and OR conditions
- Helpful to reduce “alarm noise” by creating complex composite alarms

Composite Alarm

monitor CPU ALARM

CW Alarm - A trigger

EC2 Instance

monitor IOPS ALARM

Amazon SNS

CW Alarm - B


### EC2 Instance Recover y

- Status Check:
- Instance status = check the EC2 VM
- System status = check the underlying hardware
- Attached EBS status = check attached EBS volumes

monitor alert

EC2 Instance CloudWatch Alarm SNS Topic

StatusCheckFailed_System

EC2 Instance Recovery

- Recovery: Same Private, Public, Elastic IP, metadata, placement group

### CloudWatch Alarm: good to know

- Alarms can be created based on CloudWatch Logs Metrics Filters

CloudWatch

Metric Filter

Alert

CW Logs

CW Alarm

Amazon SNS

- To test alarms and notifications, set the alarm state to Alarm using CLI

aws cloudwatch set-alarm-state --alarm-name "myalarm" --state-value

ALARM --state-reason "testing purposes"


### CloudWatch Network Synthetic Monitor

- Monitor and detects network issues between

AWS Cloud

your apps hosted on AWS and your on-

Private Subnet

premises data center

EC2 instance

- Identify any network performance

CloudWatch

degradation (e.g., packet loss, latency, jitter…)

Metrics

- No agents required to be installed
- Tests ICMP or TCP traffic to IPv4/IPv4 on-

premises destinations through Direct

DX Connection or VPN Connection

Connect or S2S VPN connections

- Publishes data to CloudWatch Metrics

Corporate Data Center

Server


### Amazon EventBridge

(formerly CloudWatch Events)

- Schedule: Cron jobs (scheduled scripts)

Schedule Every hour Trigger script on Lambda function

- Event Pattern: Event rules to react to a service doing something

IAM Root User Sign in Event SNS Topic with Email Notification

- Trigger Lambda functions, send SQS/SNS messages…

### Amazon EventBridge Rules

Example Destinations

Example Source

JSON

Lambda AWS Batch ECS Task

"version": "0",

EC2 Instance CodeBuild "id": "6a7e8feb-b491",

"detail-type": "EC2 Instance

(ex: Start Instance) (ex: failed build)

State-change Notification",

Filter events ….

(optional) }

SQS SNS Kinesis Data

Streams

S3 Event Trusted Advisor

(ex: upload object) (ex: new Finding) Amazon

EventBridge

Step CodePipeline CodeBuild

Functions

CloudTrail Schedule or Cron

(any API call) (ex: every 4 hours)

SSM EC2 Actions

etupmoC

noitargetnI

noitartsehcrO

ecnanetniaM


### Amazon EventBridge

AWS Services AWS SaaS Custom

Partner Custom

Default

Partners Apps

Event Bus Event Bus

Event Bus

- Event buses can be accessed by other AWS accounts using Resource-based Policies
- You can archive events (all/filter) sent to an event bus (indefinitely or set period)
- Ability to replay archived events

### Amazon EventBridge – Schema Registr y

- EventBridge can analyze the events in

your bus and infer the schema

- The Schema Registry allows you to

generate code for your application, that

will know in advance how data is

structured in the event bus

- Schema can be versioned

### Amazon EventBridge – Resource-based Policy

- Manage permissions for a specific Event Bus
- Example: allow/deny events from another AWS account or AWS region
- Use case: aggregate all events from your AWS Organization in a single AWS

account or AWS region

AWS Account AWS Account

(123456789012) (111122223333)

PutEvents

Lambda function

EventBridge Bus

(central-event-bus)

Allow events from another AWS account


### CloudWatch Container Insights

ECS Container EKS Container

- Collect, aggregate, summarize metrics and logs

from containers

- Available for containers on…

Metrics and logs

- Amazon Elastic Container Service (Amazon ECS)
- Amazon Elastic Kubernetes Services (Amazon EKS)
- Kubernetes platforms on EC2 CloudWatch

Container Insights

- Fargate (both for ECS and EKS)
- In Amazon EKS and Kubernetes, CloudWatch

Insights is using a containerized version of the

CloudWatch Agent to discover containers


### CloudWatch Lambda Insights

- Monitoring and troubleshooting

solution for serverless applications

running on AWS Lambda

- Collects, aggregates, and summarizes

system-level metrics including CPU

time, memory, disk, and network

- Collects, aggregates, and summarizes

diagnostic information such as cold

starts and Lambda worker shutdowns

- Lambda Insights is provided as a

Lambda Layer


### CloudWatch Contributor Insights

- Analyze log data and create time series that display

contributor data.

VPC Flow Logs

- See metrics about the top-N contributors
- The total number of unique contributors, and their usage.
- This helps you find top talkers and understand who or

what is impacting system performance.

- Works for any AWS-generated logs (VPC, DNS, etc..)

CloudWatch Logs

- For example, you can find bad hosts, identify the

heaviest network users, or find the URLs that generate

the most errors.

- You can build your rules from scratch, or you can also

use sample rules that AWS has created – leverages

CloudWatch

your CloudWatch Logs

Contributor Insights

- CloudWatch also provides built-in rules that you can

use to analyze metrics from other AWS services.

Top-10 IP addresses


### CloudWatch Application Insights

- Provides automated dashboards that show potential problems with

monitored applications, to help isolate ongoing issues

- Your applications run on Amazon EC2 Instances with select technologies only

(Java, .NET, Microsoft IIS Web Server, databases…)

- And you can use other AWS resources such as Amazon EBS, RDS, ELB, ASG,

Lambda, SQS, DynamoDB, S3 bucket, ECS, EKS, SNS, API Gateway…

- Powered by SageMaker
- Enhanced visibility into your application health to reduce the time it will take

you to troubleshoot and repair your applications

- Findings and alerts are sent to Amazon EventBridge and SSM OpsCenter

### CloudWatch Insights and Operational Visibility

- CloudWatch Container Insights
- ECS, EKS, Kubernetes on EC2, Fargate, needs agent for Kubernetes
- Metrics and logs
- CloudWatch Lambda Insights
- Detailed metrics to troubleshoot serverless applications
- CloudWatch Contributors Insights
- Find “Top-N” Contributors through CloudWatch Logs
- CloudWatch Application Insights
- Automatic dashboard to troubleshoot your application and related AWS services

### AWS CloudTrail

- Provides governance, compliance and audit for your AWS Account
- CloudTrail is enabled by default!
- Get an history of events / API calls made within your AWS Account by:
- Console
- SDK
- CLI
- AWS Services
- Can put logs from CloudTrail into CloudWatch Logs or S3
- A trail can be applied to All Regions (default) or a single Region.
- If a resource is deleted in AWS, investigate CloudTrail first!

### CloudTrail Diagram

CloudWatch Logs

CloudTrail Console

Console

Inspect & Audit

S3 Bucket

IAM Users &

IAM Roles


### CloudTrail Events

- Management Events:
- Operations that are performed on resources in your AWS account
- Examples:
- Configuring security (IAM AttachRolePolicy)
- Configuring rules for routing data (Amazon EC2 CreateSubnet)
- Setting up logging (AWS CloudTrail CreateTrail)
- By default, trails are configured to log management events.
- Can separate Read Events (that don’t modify resources) from Write Events (that may modify resources)
- Data Events:
- By default, data events are not logged (because high volume operations)
- Amazon S3 object-level activity (ex: GetObject, DeleteObject, PutObject): can separate Read and Write Events
- AWS Lambda function execution activity (the Invoke API)
- CloudTrail Insights Events:
- See next slide J

### CloudTrail Insights

- Enable CloudTrail Insights to detect unusual activity in your account:
- inaccurate resource provisioning
- hitting service limits
- Bursts of AWS IAM actions
- Gaps in periodic maintenance activity
- CloudTrail Insights analyzes normal management events to create a baseline
- And then continuously analyzes write events to detect unusual patterns
- Anomalies appear in the CloudTrail console
- Event is sent to Amazon S3
- An EventBridge event is generated (for automation needs)

CloudTrail Console

Continous analysis generate

Management Events

Insights Events

S3 Bucket

CloudTrail Insights

EventBridge event


### CloudTrail Events Retention

- Events are stored for 90 days in CloudTrail
- To keep events beyond this period, log them to S3 and use Athena

CloudTrail

Management Events

Athena

log analyze

Data Events

S3 Bucket

90 days

Insights Events

Long-term retention

retention


### Amazon EventBridge – Intercept API Calls

User

DeleteTable API Call 💥

alert

Log API call event

DynamoDB Amazon

CloudTrail SNS

EventBridge

(any API call)


### Amazon EventBridge + CloudTrail

API Call logs event

IAM CloudTrail EventBridge SNS

AssumeRole

User

IAM Role

API Call logs event

AuthorizeSecurityGroupIngress

EC2 CloudTrail EventBridge SNS

edit SG

User

Security Group

Inbound Rules


### AWS Config

- Helps with auditing and recording compliance of your AWS resources
- Helps record configurations and changes over time
- Questions that can be solved by AWS Config:
- Is there unrestricted SSH access to my security groups?
- Do my buckets have any public access?
- How has my ALB configuration changed over time?
- You can receive alerts (SNS notifications) for any changes
- AWS Config is a per-region service
- Can be aggregated across regions and accounts
- Possibility of storing the configuration data into S3 (analyzed by Athena)

### Config Rules

- Can use AWS managed config rules (over 75)
- Can make custom config rules (must be defined in AWS Lambda)
- Ex: evaluate if each EBS disk is of type gp2
- Ex: evaluate if each EC2 instance is t2.micro
- Rules can be evaluated / triggered:
- For each config change
- And / or: at regular time intervals
- AWS Config Rules does not prevent actions from happening (no deny)
- Pricing: no free tier, $0.003 per configuration item recorded per region,

$0.001 per config rule evaluation per region


### AWS Config Resource

- View compliance of a resource over time
- View configuration of a resource over time
- View CloudTrail API calls of a resource over time

### Config Rules – Remediations

- Automate remediation of non-compliant resources using SSM Automation

Documents

- Use AWS-Managed Automation Documents or create custom Automation

Documents

- Tip: you can create custom Automation Documents that invokes Lambda function
- You can set Remediation Retries if the resource is still non-compliant after auto-

remediation

expired Auto-Remediation Action

monitor trigger

(SSM Document: AWSConfigRemediation-

RevokeUnusedIAMUserCredentials)

IAM Access Key

AWS Config Retries: 5

(NON_COMPLIANT)

deactivate


### Config Rules – Notifications

- Use EventBridge to trigger notifications when AWS resources are non-

compliant

monitor

AWS Resources

trigger …

Security group …

NON_COMPLIANT

Lambda SNS SQS

AWS Config EventBridge

- Ability to send configuration changes and compliance state notifications

to SNS (all events – use SNS Filtering or filter at client-side)

AWS Resources monitor

trigger notification

Security group …

All events

(configuration changes, Admin

compliance state…) AWS Config SNS


### CloudWatch vs CloudTrail vs Config

- CloudWatch
- Performance monitoring (metrics, CPU, network, etc…) & dashboards
- Events & Alerting
- Log Aggregation & Analysis
- CloudTrail
- Record API calls made within your Account by everyone
- Can define trails for specific resources
- Global Service
- Config
- Record configuration changes
- Evaluate resources against compliance rules
- Get timeline of changes and compliance

### For an Elastic Load Balancer

- CloudWatch:
- Monitoring Incoming connections metric
- Visualize error codes as % over time
- Make a dashboard to get an idea of your load balancer performance
- Config:
- Track security group rules for the Load Balancer
- Track configuration changes for the Load Balancer
- Ensure an SSL certificate is always assigned to the Load Balancer (compliance)
- CloudTrail:
- Track who made any changes to the Load Balancer with API calls

---

## Advanced Identity in AWS


### AWS Organizations

- Global service
- Allows to manage multiple AWS accounts
- The main account is the management account
- Other accounts are member accounts
- Member accounts can only be part of one organization
- Consolidated Billing across all accounts - single payment method
- Pricing benefits from aggregated usage (volume discount for EC2, S3…)
- Shared reserved instances and Savings Plans discounts across accounts
- API is available to automate AWS account creation

### AWS Organizations

Root Organizational Unit (OU)

Management Account

OU (Dev) OU (Prod)

OU (HR) OU (Finance)

Member Accounts


### Organizational Units (OU) - Examples

Business Unit Environmental Lifecycle Project-Based

Sales Prod Project 1

Account 1 Account 1 Account 1

Project 1

Sales OU Prod OU

Sales Prod Project 1

Account 2 Account 2 Account 2

Retail Dev Project 2

Account 1 Account 1 Account 1

Management Management Management Project 2

Retail OU Dev OU

Account Account Account OU

Retail Dev Project 2

Account 2 Account 2 Account 2

Finance Test Project 3

Account 1 Account 1 Account 1

Finance Project 3

Test OU

OU OU

Finance Test Project 3

Account 2 Account 2 Account 2


### AWS Organizations

- Advantages
- Multi Account vs One Account Multi VPC
- Use tagging standards for billing purposes
- Enable CloudTrail on all accounts, send logs to central S3 account
- Send CloudWatch Logs to central logging account
- Establish Cross Account Roles for Admin purposes
- Security: Service Control Policies (SCP)
- IAM policies applied to OU or Accounts to restrict Users and Roles
- They do not apply to the management account (full admin power)
- Must have an explicit allow from the root through each OU in the direct path

to the target account (does not allow anything by default – like IAM)


### SCP Hierarchy

FullAWSAccess OU (Root)

- Management Account
- Can do anything (no SCP apply)
- Account A

Deny Athena Management Account

- Can do anything

FullAWSAccess • EXCEPT S3 (explicit Deny from

Sandbox OU)

FullAWSAccess + Deny S3 OU (Sandbox) OU (Workloads) • EXCEPT EC2 (explicit Deny)

Allow EC2 • Account B & C

- Can do anything

OU (Test)

FullAWSAccess + Deny EC2 Account A • EXCEPT S3 (explicit Deny from

Sandbox OU)

Account D

- Account D

Account B • Can access EC2

OU (Prod)

- Prod OU & Account E & F

FullAWSAccess

Account E • Can do anything

Account C

Account F


|  | OU (Sandbox) |  |
| --- | --- | --- |
|  | Account A Account B |  |
|  | Account C |  |


### SCP Examples

Blocklist and Allowlist strategies

More examples: https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_example-scps.html


### AWS Organizations – Tag Policies

- Helps you standardize tags across resources in an

AWS Organization

- Ensure consistent tags, audit tagged resources,

maintain proper resources categorization, …

- You define tag keys and their allowed values
- Helps with AWS Cost Allocation Tags and

Attribute-based Access Control

- Prevent any non-compliant tagging operations on

specified services and resources (has no effect

on resources without tags)

- Generate a report that lists all tagged/non-

compliant resources

- Use EventBridge to monitor non-compliant tags

### IAM Conditions

aws:SourceIp aws:RequestedRegion

restrict the client IP from restrict the region the

which the API calls are being made API calls are made to


### IAM Conditions

ec2:ResourceTag aws:MultiFactorAuthPresent

restrict based on tags to force MFA


### IAM for S3

- s3:ListBucket permission applies to

arn:aws:s3:::test

- => bucket level permission
- s3:GetObject, s3:PutObject,

s3:DeleteObject applies to

arn:awn:s3:::test/*

- => object level permission

### Resource Policies & aws:PrincipalOrgID

- aws:PrincipalOrgID can be used in any resource policies to restrict

access to accounts that are member of an AWS Organization

AWS Organization

(o-yyyyyyyyyy)

Member Accounts

S3 Bucket

(2022-financial-data)

User outside Organization


### IAM Roles vs Resource Based Policies

- Cross account:
- attaching a resource-based policy to a resource (example: S3 bucket policy)
- OR using a role as a proxy

User Role

Account A Account B

Amazon S3

User S3 Bucket

Account A Policy

Amazon S3


| S3 Bucket |
| --- |
| Policy |


### IAM Roles vs Resource-Based Policies

- When you assume a role (user, application or service), you give up your

original permissions and take the permissions assigned to the role

- When using a resource-based policy, the principal doesn’t have to give up his

permissions

- Example: User in account A needs to scan a DynamoDB table in Account A

and dump it in an S3 bucket in Account B.

- Supported by: Amazon S3 buckets, SNS topics, SQS queues, etc…

### Amazon EventBridge – Security

- When a rule runs, it needs

permissions on the target

- Resource-based policy: Lambda,

EventBridge

Lambda with

SNS, SQS, S3 buckets, API

Rule

Resource based Policy

Gateway…

e.g. Allow EventBridge

- IAM role: EC2 Auto Scaling, IAM Role

Systems Manager Run

Command, ECS task…

EventBridge EC2 Auto Scaling

Rule


### IAM Permission Boundaries

- IAM Permission Boundaries are supported for users and roles (not groups)
- Advanced feature to use a managed policy to set the maximum permissions

an IAM entity can get.

Example: No Permissions

IAM Permission Boundary IAM Permissions

Through IAM Policy


### IAM Permission Boundaries

- Can be used in combinations of Use cases

AWS Organizations SCP

- Delegate responsibilities to non

administrators within their permission

boundaries, for example create new IAM

users

- Allow developers to self-assign policies

and manage their own permissions, while

making sure they can’t “escalate” their

privileges (= make themselves admin)

- Useful to restrict one specific user

(instead of a whole account using

Organizations & SCP)

https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html


### IAM Policy Evaluation Logic

https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html


### Example IAM Policy

- Can you perform sqs:CreateQueue?
- Can you perform sqs:DeleteQueue?
- Can you perform

ec2:DescribeInstances?


### AWS IAM Identity Center

(successor to AWS Single Sign-On)

- One login (single sign-on) for all your
- AWS accounts in AWS Organizations
- Business cloud applications (e.g., Salesforce, Box, Microsoft 365, …)
- SAML2.0-enabled applications
- EC2 Windows Instances
- Identity providers
- Built-in identity store in IAM Identity Center
- 3rd party: Active Directory (AD), OneLogin, Okta…

### AWS IAM Identity Center – Login Flow

AWS IAM Identity Center


### AWS IAM Identity Center

AWS Cloud

AWS IAM Identity Center

Windows

login Organization

Permission Sets Business Cloud Apps

Browser Interface

Store / retrieve

User identities

Active Directory

IAM Identity Center

Custom SAML2.0-enabled Apps

Users & groups

Built-in Identity Store

(On-premises, cloud)


### IAM Identity Center

AWS Organization

IAM Identity Center

(in Management account)

Management Account

Group (Developers)

OU (Development) OU (Production)

Bob Alice

Dev Account A Prod Account A

assign assign

Dev Account B Prod Account B

Permission Set

ReadOnlyAccess

Permission Set

FullAccess


### AWS IAM Identity Center

Fine-grained Permissions and Assignments

- Multi-Account Permissions
- Manage access across AWS accounts in your AWS Organization

AWS Organization

- Permission Sets – a collection of one or more IAM Policies

assigned to users and groups to define AWS access

Dev Prod

Account Account

- Application Assignments
- SSO access to many SAML 2.0 business applications (Salesforce,

Box, Microsoft 365, …) RDS Aurora RDS Aurora

- Provide required URLs, certificates, and metadata

IAM Role IAM Role

- Attribute-Based Access Control (ABAC)

assume

- Fine-grained permissions based on users’ attributes stored in Permission Sets

IAM Identity Center Identity Store (DB Admins)

- Example: cost center, title, locale, …

Permission Sets

- Use case: Define permissions once, then modify AWS access by

(DB Admins)

changing the attributes

Database

IAM Identity Center

Admins


### What is Microsoft Active Director y (AD)?

- Found on any Windows Server

with AD Domain Services

Domain Controller

- Database of objects: User

John

Accounts, Computers, Printers,

Password

File Shares, Security Groups

- Centralized security

management, create account,

assign permissions

- Objects are organized in trees
- A group of trees is a forest

### AWS Director y Ser vices

- AWS Managed Microsoft AD

auth trust auth

- Create your own AD in AWS, manage users

locally, supports MFA

- Establish “trust” connections with your on-

premises AD

On-prem AD AWS Managed AD

- AD Connector

proxy auth

- Directory Gateway (proxy) to redirect to on-

premises AD, supports MFA

- Users are managed on the on-premises AD

On-prem AD AD Connector

- Simple AD
- AD-compatible managed directory on AWS
- Cannot be joined with on-premises AD

Simple AD


### IAM Identity Center – Active Director y Setup

- Connect to an AWS Managed Microsoft AD (Directory Service)
- Integration is out of the box

IAM Identity connect AWS Managed

Center Microsoft AD

- Connect to a Self-Managed Directory
- Create Two-way Trust Relationship using AWS Managed Microsoft AD
- Create an AD Connector

AWS Managed

Microsoft AD

connect two-way trust relationship

IAM Identity

Center connect

proxy

AD Connector


### AWS Control Tower

- Easy way to set up and govern a secure and compliant multi-account

AWS environment based on best practices

- AWS Control Tower uses AWS Organizations to create accounts
- Benefits:
- Automate the set up of your environment in a few clicks
- Automate ongoing policy management using guardrails
- Detect policy violations and remediate them
- Monitor compliance through an interactive dashboard

### AWS Control Tower – Guardrails

- Provides ongoing governance for your Control Tower environment (AWS Accounts)
- Preventive Guardrail – using SCPs (e.g., Restrict Regions across all your accounts)
- Detective Guardrail – using AWS Config (e.g., identify untagged resources)

AWS Control Tower

Guardrail trigger notify

(Detective) (NON_COMPLIANT)

AWS Config

Admin

monitor un-tagged

resources invoke

Member remediate

Accounts (add tags)

Lambda


### AWS Security & Encr yption

KMS, Encryption SDK, SSM Parameter Store


### Why encr yption?

Encr yption in flight (TLS / SSL)

- Data is encrypted before sending and decrypted after receiving
- TLS certificates help with encryption (HTTPS)
- Encryption in flight ensures no MITM (man in the middle attack) can

happen

Username: admin aGVsbG8gd29 Username: admin

Password: supersecret ybGQgZWh… Password: supersecret

TLS Encryption TLS Decryption

Client

HTTPS Website

Client Server


### Why encr yption?

Ser ver-side encr yption at rest

- Data is encrypted after being received by the server
- Data is decrypted before being sent
- It is stored in an encrypted form thanks to a key (usually a data key)
- The encryption / decryption keys must be managed somewhere, and

the server must have access to it

Encryption Decryption

HTTP(S) HTTP(S)

Object Object

Data key Data key

AWS Service (e.g., S3)


### Why encr yption?

Client-side encr yption

- Data is encrypted by the client and never decrypted by the server
- Data will be decrypted by a receiving client
- The server should not be able to decrypt the data
- Could leverage Envelope Encryption

store

Encryption

Data key

Object

(client-side)

retrieve

+ Encrypted object

Decryption

Data key

Object

(client-side)

Any storage service

(FTP, S3, …)

Client


### AWS KMS (Key Management Ser vice)

- Anytime you hear “encryption” for an AWS service, it’s most likely KMS
- AWS manages encryption keys for us
- Fully integrated with IAM for authorization
- Easy way to control access to your data
- Able to audit KMS Key usage using CloudTrail
- Seamlessly integrated into most AWS services (EBS, S3, RDS, SSM…)
- Never ever store your secrets in plaintext, especially in your code!
- KMS Key Encryption also available through API calls (SDK, CLI)
- Encrypted secrets can be stored in the code / environment variables

### KMS Keys Types

- KMS Keys is the new name of KMS Customer Master Key
- Symmetric (AES-256 keys)
- Single encryption key that is used to Encrypt and Decrypt
- AWS services that are integrated with KMS use Symmetric CMKs
- You never get access to the KMS Key unencrypted (must call KMS API to use)
- Asymmetric (RSA & ECC key pairs)
- Public (Encrypt) and Private Key (Decrypt) pair
- Used for Encrypt/Decrypt, or Sign/Verify operations
- The public key is downloadable, but you can’t access the Private Key unencrypted
- Use case: encryption outside of AWS by users who can’t call the KMS API

### AWS KMS (Key Management Ser vice)

- Types of KMS Keys:
- AWS Owned Keys (free): SSE-S3, SSE-SQS, SSE-DDB (default key)
- AWS Managed Key: free (aws/service-name, example: aws/rds or aws/ebs)
- Customer managed keys created in KMS: $1 / month
- Customer managed keys imported: $1 / month
- + pay for API call to KMS ($0.03 / 10000 calls)
- Automatic Key rotation:
- AWS-managed KMS Key: automatic every 1 year
- Customer-managed KMS Key: (must be enabled) automatic & on-demand
- Imported KMS Key: only manual rotation possible using alias

### Copying Snapshots across regions

Region eu-west-2 Region ap-southeast-2

EBS Volume EBS Volume

Encrypted Encrypted

With KMS KMS Key A With KMS KMS Key B

EBS Snapshot EBS Snapshot

Encrypted Encrypted

With KMS With KMS

KMS Key A KMS Key B

KMS ReEncrypt with KMS Key B


### KMS Key Policies

- Control access to KMS keys, “similar” to S3 bucket policies
- Difference: you cannot control access without them
- Default KMS Key Policy:
- Created if you don’t provide a specific KMS Key Policy
- Complete access to the key to the root user = entire AWS account
- Custom KMS Key Policy:
- Define users, roles that can access the KMS key
- Define who can administer the key
- Useful for cross-account access of your KMS key

### Copying Snapshots across accounts

1. Create a Snapshot, encrypted with

your own KMS Key (Customer

Managed Key)

2. Attach a KMS Key Policy to

authorize cross-account access

3. Share the encrypted snapshot

4. (in target) Create a copy of the

Snapshot, encrypt it with a CMK in

your account

5. Create a volume from the snapshot

KMS Key Policy


### KMS Multi-Region Keys

AWS KMS

us-west-2

multi-Region Replica key

arn:aws:kms:us-west-2:111122223333:

key/mrk-1234abcd12ab34cd56ef1234567890ab

us-east-1 eu-west-1

sync

multi-Region Primary key multi-Region Replica key

arn:aws:kms:us-east-1:111122223333: arn:aws:kms:eu-west-1:111122223333:

key/mrk-1234abcd12ab34cd56ef1234567890ab key/mrk-1234abcd12ab34cd56ef1234567890ab

ap-southeast-2

multi-Region Replica key

arn:aws:kms:ap-southeast-2:111122223333:

key/mrk-1234abcd12ab34cd56ef1234567890ab


### KMS Multi-Region Keys

- Identical KMS keys in different AWS Regions that can be used interchangeably
- Multi-Region keys have the same key ID, key material, automatic rotation…
- Encrypt in one Region and decrypt in other Regions
- No need to re-encrypt or making cross-Region API calls
- KMS Multi-Region are NOT global (Primary + Replicas)
- Each Multi-Region key is managed independently
- Use cases: global client-side encryption, encryption on Global DynamoDB, Global Aurora

### DynamoDB Global Tables and KMS Multi-

Region Keys Client-Side encr yption

- We can encrypt specific attributes client-side

us-east-1

in our DynamoDB table using the Amazon

DynamoDB Encryption Client 1. Encrypt attribute

with primary MRK

- Combined with Global Tables, the client-side

2. Put encrypted Attr

encrypted data is replicated to other regions

attribute (SSN)

- If we use a multi-region key, replicated in the Client App DDB Table

same region as the DynamoDB Global table,

3. Global Table

then clients in these regions can use low-

Replication

latency API calls to KMS in their region to

ap-southeast-2

decrypt the data client-side

- Using client-side encryption we can protect 4. Get encrypted Attr

attribute (SSN)

specific fields and guarantee only decryption

Client App DDB Table

if the client has access to an API key

5. Decrypt attribute

with replica MRK

replication


### Global Aurora and KMS Multi-Region Keys

Client-Side encr yption

- We can encrypt specific attributes client-side

us-east-1

in our Aurora table using the AWS KMS

Encryption SDK

1. Encrypt attribute

with primary MRK

- Combined with Aurora Global Tables, the

client-side encrypted data is replicated to 2. Put encrypted Col

other regions column (SSN)

Client App Table

- If we use a multi-region key, replicated in the

same region as the Global Aurora DB, then

3. Global DB

clients in these regions can use low-latency

Replication

API calls to KMS in their region to decrypt

the data client-side ap-southeast-2

- Using client-side encryption we can protect

4. Get encrypted Col

specific fields and guarantee only decryption column (SSN)

if the client has access to an API key, we can

Client App Table

protect specific fields even from database

5. Decrypt attribute

admins with replica MRK

replication


### S3 Replication

Encr yption Considerations

- Unencrypted objects and objects encrypted with SSE-S3 are replicated by default
- Objects encrypted with SSE-C (customer provided key) can be replicated
- For objects encrypted with SSE-KMS, you need to enable the option
- Specify which KMS Key to encrypt the objects within the target bucket
- Adapt the KMS Key Policy for the target key
- An IAM Role with kms:Decrypt for the source KMS Key and kms:Encrypt for the target KMS Key
- You might get KMS throttling errors, in which case you can ask for a Service Quotas increase
- You can use multi-region AWS KMS Keys, but they are currently treated as

independent keys by Amazon S3 (the object will still be decrypted and then

encrypted)


### AMI Sharing Process Encr ypted via KMS

1. AMI in Source Account is encrypted with KMS Key

Account - A

from Source Account KMS

2. Must modify the image attribute to add a Launch

Permission which corresponds to the specified target

AWS account

AMI Key

3. Must share the KMS Keys used to encrypted the

snapshot the AMI references with the target account

/ IAM Role

share share

4. The IAM Role/User in the target account must have

the permissions to DescribeKey, ReEncrypt*,

Account - B

CreateGrant, Decrypt

5. When launching an EC2 instance from the AMI,

launch

optionally the target account can specify a new KMS

key in its own account to re-encrypt the volumes

AMI EC2 Instance


### SSM Parameter Store

- Secure storage for configuration and secrets
- Optional Seamless Encryption using KMS

Applications

- Serverless, scalable, durable, easy SDK

Plaintext Encrypted

- Version tracking of configurations / secrets

configuration configuration

- Security through IAM
- Notifications with Amazon EventBridge SSM Parameter

Check IAM Store

permissions

- Integration with CloudFormation

Decryption

Service

AWS KMS


### SSM Parameter Store Hierarchy

- /my-department/
- my-app/

GetParameters or

- dev/ GetParametersByPath API
- db-url

Dev Lambda

- db-password

Function

- prod/
- db-url

Prod Lambda

- db-password

Function

- other-app/
- /other-department/
- /aws/reference/secretsmanager/secret_ID_in_Secrets_Manager
- /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2 (public)

### Standard and advanced parameter tiers

Standard Advanced

Total number of parameters 10,000 100,000

allowed

(per AWS account and

Region)

Maximum size of a 4 KB 8 KB

parameter value

Parameter policies available No Yes

Cost No additional charge Charges apply

Storage Pricing Free $0.05 per advanced parameter per

month


|  | Standard | Advanced |
| --- | --- | --- |
| Total number of parameters allowed (per AWS account and Region) | 10,000 | 100,000 |
| Maximum size of a parameter value | 4 KB | 8 KB |
| Parameter policies available | No | Yes |
| Cost | No additional charge | Charges apply |
| Storage Pricing | Free | $0.05 per advanced parameter per month |


### Parameters Policies (for advanced parameters)

- Allow to assign a TTL to a parameter (expiration date) to force

updating or deleting sensitive data such as passwords

- Can assign multiple policies at a time

Expiration (to delete a parameter) ExpirationNotification (EventBridge) NoChangeNotification (EventBridge)


### AWS Secrets Manager

- Newer service, meant for storing secrets
- Capability to force rotation of secrets every X days
- Automate generation of secrets on rotation (uses Lambda)
- Integration with Amazon RDS (MySQL, PostgreSQL, Aurora)
- Secrets are encrypted using KMS
- Mostly meant for RDS integration

### AWS Secrets Manager – Multi-Region Secrets

- Replicate Secrets across multiple AWS Regions
- Secrets Manager keeps read replicas in sync with the primary Secret
- Ability to promote a read replica Secret to a standalone Secret
- Use cases: multi-region apps, disaster recovery strategies, multi-region DB…

us-east-1 (Primary) us-west-2 (Secondary)

Secrets replicate Secrets

Manager Manager

MySecret-A MySecret-A

(primary) (replica)


### AWS Cer tificate Manager (ACM)

- Easily provision, manage, and deploy TLS Certificates
- Provide in-flight encryption for websites (HTTPS)

HTTPS

- Supports both public and private TLS certificates

provision and

- Free of charge for public TLS certificates

Application

maintain TLS certs

Load

- Automatic TLS certificate renewal

Balancer

- Integrations with (load TLS certificates on) AWS Certificate Manager
- Elastic Load Balancers (CLB, ALB, NLB)

HTTP

- CloudFront Distributions
- APIs on API Gateway

Auto Scaling group

- Cannot use ACM with EC2 (can’t be extracted)

EC2 Instance EC2 Instance


### ACM – Requesting Public Cer tificates

1. List domain names to be included in the certificate

- Fully Qualified Domain Name (FQDN): corp.example.com
- Wildcard Domain: *.example.com

2. Select Validation Method: DNS Validation or Email validation

- DNS Validation is preferred for automation purposes
- Email validation will send emails to contact addresses in the WHOIS database
- DNS Validation will leverage a CNAME record to DNS config (ex: Route 53)

3. It will take a few hours to get verified

4. The Public Certificate will be enrolled for automatic renewal

- ACM automatically renews ACM-generated certificates 60 days before expiry

### ACM – Impor ting Public Cer tificates

- Option to generate the certificate

outside of ACM and then import it

- No automatic renewal, must import a

ACM Events:

new certificate before expiry Daily Certificate Expiry

Lambda

- ACM sends daily expiration events

starting 45 days prior to expiration

- The # of days can be configured Rule check
- Events are appearing in EventBridge

EventBridge

- AWS Config has a managed rule

Rule events:

named acm-certificate-expiration-check Non-compliance

to check for expiring certificates

AWS Config

(configurable number of days)


### ACM – Integration with ALB

Application Load Balancer

With HTTP -> HTTPS redirect rule

HTTP

Auto Scaling group

Redirect to HTTPS

HTTPS

EC2 Instance EC2 Instance

provision and

maintain TLS certs

AWS Certificate Manager


### API Gateway - Endpoint Types

- Edge-Optimized (default): For global clients
- Requests are routed through the CloudFront Edge locations (improves latency)
- The API Gateway still lives in only one region
- Regional:
- For clients within the same region
- Could manually combine with CloudFront (more control over the caching

strategies and the distribution)

- Private:
- Can only be accessed from your VPC using an interface VPC endpoint (ENI)
- Use a resource policy to define access

### ACM – Integration with API Gateway

us-east-1

- Create a Custom Domain Name in API Gateway
- Edge-Optimized (default): For global clients

linked

- Requests are routed through the CloudFront Edge locations certificate

(improves latency)

CloudFront ACM

- The API Gateway still lives in only one region
- The TLS Certificate must be in the same region as

CloudFront, in us-east-1

API Gateway

- Then setup CNAME or (better) A-Alias record in Route 53 Edge-Optimized
- Regional:
- For clients within the same region

ap-southeast-2

- The TLS Certificate must be imported on API Gateway, in

the same region as the API Stage

linked

- Then setup CNAME or (better) A-Alias record in Route 53 certificate

API Gateway ACM

Regional


### CloudHSM

- KMS => AWS manages the software for encryption
- CloudHSM => AWS provisions encryption hardware
- Dedicated Hardware (HSM = Hardware Security Module)
- You manage your own encryption keys entirely (not AWS)
- HSM device is tamper resistant, FIPS 140-2 Level 3 compliance
- Supports both symmetric and asymmetric encryption (SSL/TLS keys)
- No free tier available
- Must use the CloudHSM Client Software
- Redshift supports CloudHSM for database encryption and key management
- Good option to use with SSE-C encryption

### CloudHSM Diagram

AWS manages the Hardware

SSL Connection

User manages the Keys

AWS CloudHSM

CloudHSM Client

IAM permissions: CloudHSM Software:

- CRUD an HSM Cluster • Manage the Keys
- Manage the Users

### CloudHSM – High Availability

- CloudHSM clusters are spread across Multi AZ (HA)
- Great for availability and durability

Availability Zone 1

CloudHSM 1

Availability Zone 2

CloudHSM Client

CloudHSM 2


### CloudHSM – Integration with AWS Ser vices

CloudHSM

- Through integration with

AWS KMS

- Configure KMS Custom

Connector

Key Store with

RDS DB

CloudHSM Instance

KMS Encryption

EBS Volume

- Example: EBS, S3, RDS …

AWS KMS

(Custom Key Store)

CloudTrail

keys usage logs


### CloudHSM vs. KMS

Feature AWS KMS AWS CloudHSM

Tenancy Multi-Tenant Single-Tenant

Standard FIPS 140-2 Level 3 FIPS 140-2 Level 3

Master Keys • AWS Owned CMK Customer Managed CMK

- AWS Managed CMK
- Customer Managed CMK

Key Types • Symmetric • Symmetric

- Asymmetric • Asymmetric
- Digital Signing • Digital Signing & Hashing

Key Accessibility Accessible in multiple AWS regions (can’t • Deployed and managed in a VPC

access keys outside the region it’s created in) • Can be shared across VPCs (VPC Peering)

Cryptographic None • SSL/TLS Acceleration

Acceleration • Oracle TDE Acceleration

Access & AWS IAM You create users and manage their permissions

Authentication


| Feature | AWS KMS | AWS CloudHSM |
| --- | --- | --- |
| Tenancy | Multi-Tenant | Single-Tenant |
| Standard | FIPS 140-2 Level 3 | FIPS 140-2 Level 3 |
| Master Keys | • AWS Owned CMK • AWS Managed CMK • Customer Managed CMK | Customer Managed CMK |
| Key Types | • Symmetric • Asymmetric • Digital Signing | • Symmetric • Asymmetric • Digital Signing & Hashing |
| Key Accessibility | Accessible in multiple AWS regions (can’t access keys outside the region it’s created in) | • Deployed and managed in a VPC • Can be shared across VPCs (VPC Peering) |
| Cryptographic Acceleration | None | • SSL/TLS Acceleration • Oracle TDE Acceleration |
| Access & Authentication | AWS IAM | You create users and manage their permissions |


### CloudHSM vs. KMS

Feature AWS KMS AWS CloudHSM

High Availability AWS Managed Service Add multiple HSMs over different AZs

Audit Capability • CloudTrail • CloudTrail

- CloudWatch • CloudWatch
- MFA support

Free Tier Yes No


| Feature | AWS KMS | AWS CloudHSM |
| --- | --- | --- |
| High Availability | AWS Managed Service | Add multiple HSMs over different AZs |
| Audit Capability | • CloudTrail • CloudWatch | • CloudTrail • CloudWatch • MFA support |
| Free Tier | Yes | No |


### AWS WAF – Web Application Firewall

- Protects your web applications from common web exploits (Layer 7)
- Layer 7 is HTTP (vs Layer 4 is TCP/UDP)
- Deploy on
- Application Load Balancer
- API Gateway
- CloudFront
- AppSync GraphQL API
- Cognito User Pool

### AWS WAF – Web Application Firewall

- Define Web ACL (Web Access Control List) Rules:
- IP Set: up to 10,000 IP addresses – use multiple Rules for more IPs
- HTTP headers, HTTP body, or URI strings Protects from common attack - SQL

injection and Cross-Site Scripting (XSS)

- Size constraints, geo-match (block countries)
- Rate-based rules (to count occurrences of events) – for DDoS protection
- Web ACL are Regional except for CloudFront
- A rule group is a reusable set of rules that you can add to a web ACL

### WAF – Fixed IP while using WAF with a Load

Balancer

- WAF does not support the Network Load Balancer (Layer 4)
- We can use Global Accelerator for fixed IP and WAF on the ALB

us-east-1

Application Load

Balancer

Users

EC2 Instances

Global Accelerator

attached

Fixed IPv4: 1.2.3.4

WebACL

WebACL must be in the same

AWS WAF

AWS Region as ALB


### AWS Shield: protect from DDoS attack

- DDoS: Distributed Denial of Service – many requests at the same time
- AWS Shield Standard:
- Free service that is activated for every AWS customer
- Provides protection from attacks such as SYN/UDP Floods, Reflection attacks and other

layer 3/layer 4 attacks

- AWS Shield Advanced:
- Optional DDoS mitigation service ($3,000 per month per organization)
- Protect against more sophisticated attack on Amazon EC2, Elastic Load Balancing (ELB),

Amazon CloudFront, AWS Global Accelerator, and Route 53

- 24/7 access to AWS DDoS response team (DRP)
- Protect against higher fees during usage spikes due to DDoS
- Shield Advanced automatic application layer DDoS mitigation automatically creates,

evaluates and deploys AWS WAF rules to mitigate layer 7 attacks


### AWS Firewall Manager

- Manage rules in all accounts of an AWS Organization
- Security policy: common set of security rules
- WAF rules (Application Load Balancer, API Gateways, CloudFront)
- AWS Shield Advanced (ALB, CLB, NLB, Elastic IP, CloudFront)
- Security Groups for EC2, Application Load BAlancer and ENI resources in VPC
- AWS Network Firewall (VPC Level)
- Amazon Route 53 Resolver DNS Firewall
- Policies are created at the region level
- Rules are applied to new resources as they are created (good for

compliance) across all and future accounts in your Organization


### WAF vs. Firewall Manager vs. Shield

AWS WAF AWS Firewall Manager AWS Shield

- WAF, Shield and Firewall Manager are used together for comprehensive protection
- Define your Web ACL rules in WAF
- For granular protection of your resources, WAF alone is the correct choice
- If you want to use AWS WAF across accounts, accelerate WAF configuration,

automate the protection of new resources, use Firewall Manager with AWS WAF

- Shield Advanced adds additional features on top of AWS WAF, such as dedicated

support from the Shield Response Team (SRT) and advanced reporting.

- If you’re prone to frequent DDoS attacks, consider purchasing Shield Advanced

### AWS Best Practices for DDoS Resiliency

Edge Location Mitigation (BP1, BP3)

- BP1 – CloudFront
- Web Application delivery at

the edge

- Protect from DDoS Common

Attacks (SYN floods, UDP

reflection…)

- BP1 – Global Accelerator
- Access your application from

the edge

- Integration with Shield for

DDoS protection

- Helpful if your backend is not

compatible with CloudFront

- BP3 – Route 53
- Domain Name Resolution at

the edge

- DDoS Protection mechanism

### AWS Best Practices for DDoS Resiliency

Best pratices for DDoS mitigation

- Infrastructure layer defense (BP1,

BP3, BP6)

- Protect Amazon EC2 against high

traffic

- That includes using Global

Accelerator, Route 53,

CloudFront, Elastic Load Balancing

- Amazon EC2 with Auto Scaling

(BP7)

- Helps scale in case of sudden

traffic surges including a flash

crowd or a DDoS attack

- Elastic Load Balancing (BP6)
- Elastic Load Balancing scales with

the traffic increases and will

distribute the traffic to many EC2

instances


### AWS Best Practices for DDoS Resiliency

Application Layer Defense

- Detect and filter malicious web

requests (BP1, BP2)

- CloudFront cache static content and

serve it from edge locations, protecting

your backend

- AWS WAF is used on top of

CloudFront and Application Load

Balancer to filter and block requests

based on request signatures

- WAF rate-based rules can

automatically block the IPs of bad

actors

- Use managed rules on WAF to block

attacks based on IP reputation, or

block anonymous Ips

- CloudFront can block specific

geographies

- Shield Advanced (BP1, BP2, BP6)
- Shield Advanced automatic application

layer DDoS mitigation automatically

creates, evaluates and deploys AWS

WAF rules to mitigate layer 7 attacks


### AWS Best Practices for DDoS Resiliency

Attack surface reduction

- Obfuscating AWS resources (BP1,

BP4, BP6)

- Using CloudFront, API Gateway, Elastic

Load Balancing to hide your backend

resources (Lambda functions, EC2

instances)

- Security groups and Network ACLs

(BP5)

- Use security groups and NACLs to

filter traffic based on specific IP at the

subnet or ENI-level

- Elastic IP are protected by AWS Shield

Advanced

- Protecting API endpoints (BP4)
- Hide EC2, Lambda, elsewhere
- Edge-optimized mode, or CloudFront

+ regional mode (more control for

DDoS)

- WAF + API Gateway: burst limits,

headers filtering, use API keys


### Amazon GuardDuty

- Intelligent Threat discovery to protect your AWS Account
- Uses Machine Learning algorithms, anomaly detection, 3rd party data
- One click to enable (30 days trial), no need to install software
- Input data includes:
- CloudTrail Events Logs – unusual API calls, unauthorized deployments
- CloudTrail Management Events – create VPC subnet, create trail, …
- CloudTrail S3 Data Events – get object, list objects, delete object, …
- VPC Flow Logs – unusual internal traffic, unusual IP address
- DNS Logs – compromised EC2 instances sending encoded data within DNS queries
- Optional Features – EKS Audit Logs, RDS & Aurora, EBS, Lambda, S3 Data Events…
- Can setup EventBridge rules to be notified in case of findings
- EventBridge rules can target AWS Lambda or SNS
- Can protect against CryptoCurrency attacks (has a dedicated “finding” for it)

### Amazon GuardDuty

VPC Flow Logs

CloudTrail Logs

DNS Logs (AWS DNS) SNS

GuardDuty

Optional Features

S3 Logs EBS Volumes

EventBridge

Lambda

Lambda Network RDS & Aurora

Activity Login Activity

EKS Audit Logs &

Runtime Monitoring


### Amazon Inspector

SSM Agent

- Automated Security Assessments Lambda

Function

- For EC2 instances
- Leveraging the AWS System Manager (SSM) agent
- Analyze against unintended network accessibility
- Analyze the running OS against known vulnerabilities

Amazon

- For Container Images push to Amazon ECR

Inspector

- Assessment of Container Images as they are pushed

Amazon ECR

- For Lambda Functions

Container Image

- Identifies software vulnerabilities in function code and package

dependencies

- Assessment of functions as they are deployed assessment run state

& findings

- Reporting & integration with AWS Security Hub
- Send findings to Amazon Event Bridge

Security Hub EventBridge


### What does Amazon Inspector evaluate?

- Remember: only for EC2 instances, Container Images & Lambda functions
- Continuous scanning of the infrastructure, only when needed
- Package vulnerabilities (EC2, ECR & Lambda) – database of CVE
- Network reachability (EC2)
- A risk score is associated with all vulnerabilities for prioritization

### AWS Macie

- Amazon Macie is a fully managed data security and data privacy service

that uses machine learning and pattern matching to discover and

protect your sensitive data in AWS.

- Macie helps identify and alert you to sensitive data, such as personally

identifiable information (PII)

analyze notify integrations

S3 Buckets Macie Amazon

Discover Sensitive Data (PII) EventBridge


---

## Amazon VPC


### VPC Components Diagram

Amazon

DynamoDB

VPC Flow Logs

Region

NACL NACL

Internet

Corporate

www Public Subnet Private Subnet Data Center

CloudWatch

Security Group

Internet Router

Gateway

NAT Gateway

Transit Private EC2 Instance S3

Gateway

Route Route Server

VPN Customer

Table Security Group Table Security Group VPC Gateway

Endpoint

S2S VPN

Connection

VPC Peering Public EC2 Instance Private EC2 Instance

Connections VPN

Gateway

Direct Connect

Availability Zone

Connection

DX Location


|  |  |  |
| --- | --- | --- |
| VPC rnet Router way VPN DX | NACL NACL Public Subnet Private Subnet Security Group NAT Gateway Private EC2 Instance Route Route Table Security Group Table Security Group Public EC2 Instance Private EC2 Instance | VPC Endpoin VP Gate |
|  | Availability Zone |  |


### Understanding CIDR – IPv4

- Classless Inter-Domain Routing – a method for allocating IP addresses
- Used in Security Groups rules and AWS networking in general
- They help to define an IP address range:
- We’ve seen WW.XX.YY.ZZ/32 => one IP
- We’ve seen 0.0.0.0/0 => all IPs
- But we can define:192.168.0.0/26 =>192.168.0.0 – 192.168.0.63 (64 IP addresses)

### Understanding CIDR – IPv4

- A CIDR consists of two components
- Base IP
- Represents an IP contained in the range (XX.XX.XX.XX)
- Example: 10.0.0.0, 192.168.0.0, …
- Subnet Mask
- Defines how many bits can change in the IP
- Example: /0, /24, /32
- Can take two forms:
- /8 ó 255.0.0.0
- /16 ó 255.255.0.0
- /24 ó 255.255.255.0
- /32 ó 255.255.255.255

### Understanding CIDR – Subnet Mask

- The Subnet Mask basically allows part of the underlying IP to get

additional next values from the base IP

192 . 168 . 0 . 0 /32 => allows for 1 IP (2!) 192.168.0.0

192 . 168 . 0 . 0 /31 => allows for 2 IP (2") 192.168.0.0 -> 192.168.0.1 Quick Memo

192 . 168 . 0 . 0 /30 => allows for 4 IP (2#) 192.168.0.0 -> 192.168.0.3

192 . 168 . 0 . 0 /29 => allows for 8 IP (2$) 192.168.0.0 -> 192.168.0.7 Octets

. . .

192 . 168 . 0 . 0 /28 => allows for 16 IP (2%) 192.168.0.0 -> 192.168.0.15 1*+ 2,- 3.- 4+/

192 . 168 . 0 . 0 /27 => allows for 32 IP (2&) 192.168.0.0 -> 192.168.0.31

192 . 168 . 0 . 0 /26 => allows for 64 IP (2') 192.168.0.0 -> 192.168.0.63 • /32 – no octet can change

192 . 168 . 0 . 0 /25 => allows for 128 IP (2() 192.168.0.0 -> 192.168.0.127 • /24 – last octet can change

192 . 168 . 0 . 0 /24 => allows for 256 IP (2)) 192.168.0.0 -> 192.168.0.255 • /16 – last 2 octets can change

… • /8 – last 3 octets can change

- /0 – all octets can change

192 . 168 . 0 . 0 /16 => allows for 65,536 IP (2"') 192.168.0.0 -> 192.168.255.255

0 . 0 . 0 . 0 /0 => allows for All IPs 0.0.0.0 -> 255.255.255.255


### Understanding CIDR – Little Exercise

- 192.168.0.0/24 = … ?
- 192.168.0.0 – 192.168.0.255 (256 IPs)
- 192.168.0.0/16 = … ?
- 192.168.0.0 – 192.168.255.255 (65,536 IPs)
- 134.56.78.123/32 = … ?
- Just 134.56.78.123
- 0.0.0.0/0
- All IPs!
- When in doubt, use this website https://www.ipaddressguide.com/cidr

### Public vs. Private IP (IPv4)

- The Internet Assigned Numbers Authority (IANA) established certain

blocks of IPv4 addresses for the use of private (LAN) and public

(Internet) addresses

- Private IP can only allow certain values:
- 10.0.0.0 – 10.255.255.255 (10.0.0.0/8) ç in big networks
- 172.16.0.0 – 172.31.255.255 (172.16.0.0/12) ç AWS default VPC in that range
- 192.168.0.0 – 192.168.255.255 (192.168.0.0/16) ç e.g., home networks
- All the rest of the IP addresses on the Internet are Public

### Default VPC Walkthrough

- All new AWS accounts have a default VPC
- New EC2 instances are launched into the default VPC if no subnet is

specified

- Default VPC has Internet connectivity and all EC2 instances inside it

have public IPv4 addresses

- We also get a public and a private IPv4 DNS names

### VPC in AWS – IPv4

- VPC = Virtual Private Cloud
- You can have multiple VPCs in an AWS region (max. 5 per region – soft limit)
- Max. CIDR per VPC is 5, for each CIDR:
- Min. size is /28 (16 IP addresses)
- Max. size is /16 (65536 IP addresses)
- Because VPC is private, only the Private IPv4 ranges are allowed:
- 10.0.0.0 – 10.255.255.255 (10.0.0.0/8)
- 172.16.0.0 – 172.31.255.255 (172.16.0.0/12)
- 192.168.0.0 – 192.168.255.255 (192.168.0.0/16)
- Your VPC CIDR should NOT overlap with your other networks (e.g., corporate)

### State of Hands-on

Region


### Adding Subnets

Region

Public Subnet Private Subnet

Availability Zone


|  |  |  |
| --- | --- | --- |
| VPC | Public Subnet Private Subnet |  |
|  | Availability Zone |  |


### VPC – Subnet (IPv4)

- AWS reserves 5 IP addresses (first 4 & last 1) in each subnet
- These 5 IP addresses are not available for use and can’t be assigned to an

EC2 instance

- Example: if CIDR block 10.0.0.0/24, then reserved IP addresses are:
- 10.0.0.0 – Network Address
- 10.0.0.1 – reserved by AWS for the VPC router
- 10.0.0.2 – reserved by AWS for mapping to Amazon-provided DNS
- 10.0.0.3 – reserved by AWS for future use
- 10.0.0.255 – Network Broadcast Address. AWS does not support broadcast in a VPC,

therefore the address is reserved

- Exam Tip, if you need 29 IP addresses for EC2 instances:
- You can’t choose a subnet of size /27 (32 IP addresses, 32 – 5 = 27 < 29)
- You need to choose a subnet of size /26 (64 IP addresses, 64 – 5 = 59 > 29)

### Internet Gateway (IGW)

- Allows resources (e.g., EC2 instances) in a VPC connect to the Internet
- It scales horizontally and is highly available and redundant
- Must be created separately from a VPC
- One VPC can only be attached to one IGW and vice versa
- Internet Gateways on their own do not allow Internet access…
- Route tables must also be edited!

### Adding Internet Gateway

Region

Public Subnet Private Subnet

Internet

Gateway

Availability Zone


|  |  |  |
| --- | --- | --- |
| VPC rnet way | Public Subnet Private Subnet |  |
|  | Availability Zone |  |


### Editing Route Tables

Region

Internet

Public Subnet Private Subnet

Internet Router

Gateway

Route

Table Security Group

Public EC2 Instance

Availability Zone


|  |  |  |
| --- | --- | --- |
| VPC rnet Router way | Public Subnet Private Subnet Route Table Security Group Public EC2 Instance |  |
|  | Availability Zone |  |


### Bastion Hosts

Users

- We can use a Bastion Host to SSH into

our private EC2 instances

- The bastion is in the public subnet which is

then connected to all other private subnets

Public Subnet

- Bastion Host security group must allow

Security Group (BastionHost-SG)

inbound from the internet on port 22 from

EC2 Instance

restricted CIDR, for example the public

(Bastion Host)

CIDR of your corporation

Private Subnet

- Security Group of the EC2 Instances must

allow the Security Group of the Bastion Security Group (LinuxInstance-SG)

Host, or the private IP of the Bastion host


|  |  |
| --- | --- |


### NAT Instance (outdated, but still at the exam)

Server

- NAT = Network Address Translation

(IP: 50.60.4.10)

- Allows EC2 instances in private subnets to

Src.: 50.60.4.10

connect to the Internet

Dest.: 12.34.56.78

- Must be launched in a public subnet VPC Dest.: 50.60.4.10

Src.: 12.34.56.78

- Must disable EC2 setting: Source /

Public Subnet

destination Check

Security Group (NATInstance-SG)

- Must have Elastic IP attached to it

(IP: 12.34.56.78)

NAT Instance

- Route Tables must be configured to route Dest.: 50.60.4.10

Src.: 50.60.4.10

Src.: 10.0.0.20

traffic from private subnets to the NAT

Dest.: 10.0.0.20

Instance Private Subnet

IP: 10.0.0.10 IP: 10.0.0.20


| Public Subnet Security Group (NATInstance-SG) EIP (IP: 12.34.56.78) NAT Instance Dest.: 50.60.4.1 |  |  |
| --- | --- | --- |
|  | Dest.: 50.60.4.1 | 0 |


### NAT Instance

Region

Internet

Public Subnet Private Subnet

Security Group Security Group

Internet Router

Gateway

NAT Instance Private EC2 Instance

Route Route

Table Security Group Table

Public EC2 Instance

Availability Zone


|  |  |  |
| --- | --- | --- |
| VPC rnet Router way | Public Subnet Private Subnet Security Group Security Group EIP NAT Instance Private EC2 Instance Route Route Table Security Group Table Public EC2 Instance |  |
|  | Availability Zone |  |


### NAT Instance – Comments

- Pre-configured Amazon Linux AMI is available
- Reached the end of standard support on December 31, 2020
- Not highly available / resilient setup out of the box
- You need to create an ASG in multi-AZ + resilient user-data script
- Internet traffic bandwidth depends on EC2 instance type
- You must manage Security Groups & rules:
- Inbound:
- Allow HTTP / HTTPS traffic coming from Private Subnets
- Allow SSH from your home network (access is provided through Internet Gateway)
- Outbound:
- Allow HTTP / HTTPS traffic to the Internet

### NAT Gateway

- AWS-managed NAT, higher bandwidth, high availability, no administration
- Pay per hour for usage and bandwidth
- NATGW is created in a specific Availability Zone, uses an Elastic IP
- Can’t be used by EC2 instance in the same subnet (only from other

subnets)

- Requires an IGW (Private Subnet => NATGW => IGW)
- 5 Gbps of bandwidth with automatic scaling up to 100 Gbps
- No Security Groups to manage / required

### NAT Gateway

Region

Internet

Public Subnet Private Subnet

Security Group

Internet Router

Gateway

NAT Gateway

Private EC2 Instance

Route Route

Table Security Group Table

Public EC2 Instance

Availability Zone


|  |  |  |
| --- | --- | --- |
| VPC rnet Router way | Public Subnet Private Subnet Security Group NAT Gateway Private EC2 Instance Route Route Table Security Group Table Public EC2 Instance |  |
|  | Availability Zone |  |


### NAT Gateway with High Availability

Internet

- NAT Gateway is resilient within a

single Availability Zone

Region

Internet

Gateway

- Must create multiple NAT

Gateways in multiple AZs for Router

Public Subnet Public Subnet

fault-tolerance

NAT Gateway NAT Gateway

- There is no cross-AZ failover Private Subnet Private Subnet

needed because if an AZ goes

down it doesn't need NAT

EC2 Instance EC2 Instance

AZ - A AZ - B


### NAT Gateway vs. NAT Instance

NAT Gateway NAT Instance

Availability Highly available within AZ (create in another AZ) Use a script to manage failover between instances

Bandwidth Up to 100 Gbps Depends on EC2 instance type

Maintenance Managed by AWS Managed by you (e.g., software, OS patches, …)

Cost Per hour & amount of data transferred Per hour, EC2 instance type and size, + network $

Public IPv4

Private IPv4

Security Groups

Use as Bastion Host?

More at: https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-comparison.html


|  | NAT Gateway | NAT Instance |
| --- | --- | --- |
| Availability | Highly available within AZ (create in another AZ) | Use a script to manage failover between instances |
| Bandwidth | Up to 100 Gbps | Depends on EC2 instance type |
| Maintenance | Managed by AWS | Managed by you (e.g., software, OS patches, …) |
| Cost | Per hour & amount of data transferred | Per hour, EC2 instance type and size, + network $ |
| Public IPv4 |  |  |
| Private IPv4 |  |  |
| Security Groups |  |  |
| Use as Bastion Host? |  |  |


### Security Groups & NACLs

Subnet

Security Group

EC2 Instance

LCAN

Incoming Request

Subnet

NACL Inbound SG Inbound Security Group

Rules Rules

NACL Outbound

Outbound Allowed EC2 Instance Rules (Stateless)

(Stateful)

LCAN

Outgoing Request

NACL Outbound SG Outbound

Rules Rules

NACL Inbound

Inbound Allowed Rules (Stateless)

(Stateful)


### Network Access Control List (NACL)

- NACL are like a firewall which control traffic from and to subnets
- One NACL per subnet, new subnets are assigned the Default NACL
- You define NACL Rules:
- Rules have a number (1-32766), higher precedence with a lower number
- First rule match will drive the decision
- Example: if you define #100 ALLOW 10.0.0.10/32 and #200 DENY 10.0.0.10/32, the IP

address will be allowed because 100 has a higher precedence over 200

- The last rule is an asterisk (*) and denies a request in case of no rule match
- AWS recommends adding rules by increment of 100
- Newly created NACLs will deny everything
- NACL are a great way of blocking a specific IP address at the subnet level

### NACLs

Region

NACL NACL

Internet

Public Subnet Private Subnet

Security Group

Internet Router

Gateway

NAT Gateway

Private EC2 Instance

Route Route

Table Security Group Table

Public EC2 Instance

Availability Zone


|  |  |  |
| --- | --- | --- |
| VPC rnet Router way | NACL NACL Public Subnet Private Subnet Security Group NAT Gateway Private EC2 Instance Route Route Table Security Group Table Public EC2 Instance |  |
|  | Availability Zone |  |


### Default NACL

- Accepts everything inbound/outbound with the subnets it’s associated with
- Do NOT modify the Default NACL, instead create custom NACLs

Default NACL for a VPC that supports IPv4

Inbound Rules

Rule # Type Protocol Port Range Source Allow/Deny

100 All IPv4 Traffic All All 0.0.0.0/0 ALLOW

* All IPv4 Traffic All All 0.0.0.0/0 DENY

Outbound Rules

Rule # Type Protocol Port Range Destination Allow/Deny

100 All IPv4 Traffic All All 0.0.0.0/0 ALLOW

* All IPv4 Traffic All All 0.0.0.0/0 DENY


| Rule # | Type | Protocol | Port Range | Source | Allow/Deny |
| --- | --- | --- | --- | --- | --- |
| 100 | All IPv4 Traffic | All | All | 0.0.0.0/0 | ALLOW |
| * | All IPv4 Traffic | All | All | 0.0.0.0/0 | DENY |


| Rule # | Type | Protocol | Port Range | Destination | Allow/Deny |
| --- | --- | --- | --- | --- | --- |
| 100 | All IPv4 Traffic | All | All | 0.0.0.0/0 | ALLOW |
| * | All IPv4 Traffic | All | All | 0.0.0.0/0 | DENY |


### Ephemeral Por ts

- For any two endpoints to establish a connection, they must use ports
- Clients connect to a defined port, and expect a response on an ephemeral port
- Different Operating Systems use different port ranges, examples:
- IANA & MS Windows 10 è 49152 – 65535
- Many Linux Kernels è 32768 – 60999

Request

Src. IP Src. Port Dest. IP Dest. Port

Payload …

11.22.33.44 50105 55.66.77.88 443

Dest. IP Dest. Port Src. IP Src. Port

Client Payload … Web Server

11.22.33.44 50105 55.66.77.88 443

IP: 11.22.33.44 IP: 55.66.77.88

Ephemeral Port: 50105 Response Fixed Port: 443


### NACL with Ephemeral Por ts

Web Tier Database Tier

Web Subnet (Public) DB Subnet (Private)

DB Instance

Port 3306

Web-NACL

LCAN-BD

Allow Outbound TCP Allow Inbound TCP

On port 3306 On port 3306

To DB Subnet CIDR From Web Subnet CIDR

Client

Allow Inbound TCP Allow Outbound TCP

Ephemeral

On port 1024-65535 On port 1024-65535

Port

From DB Subnet CIDR To Web Subnet CIDR

https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html#nacl-ephemeral-ports


|  |  |  |  |  |
| --- | --- | --- | --- | --- |
|  | Web-NACL | Allow Inbound TCP Allow Outbound TCP On port 1024-65535 On port 1024-65535 From DB Subnet CIDR To Web Subnet CIDR | LCAN-BD |  |
|  |  |  |  |  |


### Create NACL rules for each

target subnets CIDR

Web Tier Database Tier

Web Subnet - A (Public) DB Subnet – A (Private)

DB Instance

Web Subnet - B (Public) DB Subnet – B (Private)

DB Instance

Web-NACL

LCAN-BD


|  |  |  |
| --- | --- | --- |
| Web-NACL |  | LCAN-BD |
|  |  |  |


### Security Group vs. NACLs

Security Group NACL

Operates at the instance level Operates at the subnet level

Supports allow rules only Supports allow rules and deny rules

Stateful: return traffic is automatically allowed, Stateless: return traffic must be explicitly allowed by

regardless of any rules rules (think of ephemeral ports)

All rules are evaluated before deciding whether to Rules are evaluated in order (lowest to highest) when

allow traffic deciding whether to allow traffic, first match wins

Applies to an EC2 instance when specified by Automatically applies to all EC2 instances in the

someone subnet that it’s associated with

NACL Examples: https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html


| Security Group | NACL |
| --- | --- |
| Operates at the instance level | Operates at the subnet level |
| Supports allow rules only | Supports allow rules and deny rules |
| Stateful: return traffic is automatically allowed, regardless of any rules | Stateless: return traffic must be explicitly allowed by rules (think of ephemeral ports) |
| All rules are evaluated before deciding whether to allow traffic | Rules are evaluated in order (lowest to highest) when deciding whether to allow traffic, first match wins |
| Applies to an EC2 instance when specified by someone | Automatically applies to all EC2 instances in the subnet that it’s associated with |


### VPC Peering

- Privately connect two VPCs using AWS’

network VPC - A

- Make them behave as if they were in the

same network

VPC Peering

(A – B)

- Must not have overlapping CIDRs
- VPC Peering connection is NOT transitive

VPC Peering

VPC - B

(must be established for each VPC that (A – C)

need to communicate with one another)

VPC Peering

- You must update route tables in each VPC’s

(B – C)

subnets to ensure EC2 instances can

communicate with each other

VPC - C


### VPC Peering – Good to know

- You can create VPC Peering connection between VPCs in different AWS

accounts/regions

- You can reference a security group in a peered VPC (works cross

accounts – same region)

Account ID


### VPC Peering

Region

NACL NACL

Internet

Public Subnet Private Subnet

Security Group

Internet Router

Gateway

NAT Gateway

Private EC2 Instance

Route Route

Table Security Group Table

VPC Peering Public EC2 Instance

Connections

Availability Zone


|  |  |  |
| --- | --- | --- |
| VPC rnet Router way | NACL NACL Public Subnet Private Subnet Security Group NAT Gateway Private EC2 Instance Route Route Table Security Group Table Public EC2 Instance |  |
|  | Availability Zone |  |


### VPC Endpoints

Amazon

DynamoDB

Region

NACL NACL

Internet

Public Subnet Private Subnet

CloudWatch

Security Group

Internet Router

Gateway

NAT Gateway

Private EC2 Instance S3

Route Route

Table Security Group Table VPC

Endpoint

VPC Peering Public EC2 Instance

Connections

Availability Zone


|  |  |  |
| --- | --- | --- |
| VPC rnet Router way | NACL NACL Public Subnet Private Subnet Security Group NAT Gateway Private EC2 Instance Route Route Table Security Group Table Public EC2 Instance | VPC Endpoin |
|  | Availability Zone |  |


### VPC Endpoints (AWS PrivateLink)

Amazon SNS

- Every AWS service is publicly exposed

(public URL)

- VPC Endpoints (powered by AWS

Region

Internet

PrivateLink) allows you to connect to AWS Gateway

services using a private network instead of

Public Subnet

using the public Internet

EC2 Instance

- They’re redundant and scale horizontally Gateway
- They remove the need of IGW, NATGW, … Private Subnet Option 1

to access AWS Services

EC2 Instance

Option 2

- In case of issues:

VPC Endpoint

- Check DNS Setting Resolution in your VPC
- Check Route Tables

Amazon SNS


### Types of Endpoints

Region

- Interface Endpoints (powered by PrivateLink) VPC

Private Subnet

- Provisions an ENI (private IP address) as an entry

VPC Endpoint

point (must attach a Security Group) (Interface)

EC2 Instance

ENI (PrivateLink)

- Supports most AWS services
- $ per hour + $ per GB of data processed

Amazon SNS

- Gateway Endpoints
- Provisions a gateway and must be used as a

Region

target in a route table (does not use security

groups)

Private Subnet

- Supports both S3 and DynamoDB

VPC Endpoint

- Free EC2 Instance

(Gateway)

Amazon

Amazon S3 OR

DynamoDB


### Gateway or Interface Endpoint for S3?

Users

- Gateway is most likely going to be

preferred all the time at the exam

- Cost: free for Gateway, $ for

AWS Cloud

interface endpoint S2S VPN Direct Connect

Region

- Interface Endpoint is preferred

access is required from on-

Interface

premises (Site to Site VPN or

In-VPC

Endpoint

Apps

Direct Connect), a different VPC

or a different region

Gateway

Endpoint

PrivateLink Amazon S3


### Lambda in VPC accessing DynamoDB

- DynamoDB is a public service

AWS Cloud

from AWS

Public subnet

- Option 1: Access from the public

DynamoDB

internet

NAT IGW

- Because Lambda is in a VPC, it

needs a NAT Gateway in a public

subnet and an internet gateway

Private subnet

- Option 2 (better & free): Access

VPC Gateway Endpoint

from the private VPC network

For DynamoDB

- Deploy a VPC Gateway endpoint

for DynamoDB

- Change the Route Tables

### VPC Flow Logs

- Capture information about IP traffic going into your interfaces:
- VPC Flow Logs
- Subnet Flow Logs
- Elastic Network Interface (ENI) Flow Logs
- Helps to monitor & troubleshoot connectivity issues
- Flow logs data can go to S3, CloudWatch Logs, and Kinesis Data Firehose
- Captures network information from AWS managed interfaces too: ELB,

RDS, ElastiCache, Redshift, WorkSpaces, NATGW, Transit Gateway…


### VPC Flow Logs

VPC Flow Logs

Region

NACL NACL

Internet

Public Subnet Private Subnet

CloudWatch

Security Group

Internet Router

Gateway

NAT Gateway

Amazon

Private EC2 Instance S3

DynamoDB

Route Route

Table Security Group Table VPC

Endpoint

VPC Peering Public EC2 Instance

Connections

Availability Zone


|  |  |  |
| --- | --- | --- |
| VPC rnet Router way | NACL NACL Public Subnet Private Subnet Security Group NAT Gateway Private EC2 Instance Route Route Table Security Group Table Public EC2 Instance | VPC Endpoin |
|  | Availability Zone |  |


### VPC Flow Logs Syntax

version interface-id dstaddr dstport packets start action

account-id srcaddr srcport protocol bytes end log-status

- srcaddr & dstaddr – help identify problematic IP
- srcport & dstport – help identity problematic ports
- Action – success or failure of the request due to Security Group / NACL
- Can be used for analytics on usage patterns, or malicious behavior
- Query VPC flow logs using Athena on S3 or CloudWatch Logs Insights
- Flow Logs examples: https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-

records-examples.html


### VPC Flow Logs – Troubleshoot SG & NACL issues

Subnet

Security Group

EC2 Instance

LCAN

Subnet

NACL Inbound SG Inbound Security Group

Rules Rules

NACL Outbound Outbound Allowed

Rules (Stateless) EC2 Instance (Stateful)

LCAN

Look at the “ACTION” field

Incoming Requests Outgoing Requests

- Inbound REJECT => NACL or SG • Outbound REJECT => NACL or SG
- Inbound ACCEPT, Outbound REJECT => • Outbound ACCEPT, Inbound REJECT =>

NACL NACL

NACL Outbound SG Outbound

Rules Rules

NACL Inbound Inbound Allowed

Rules (Stateless) (Stateful)


| LCAN |
| --- |
|  |


|  |
| --- |
| LCAN |


### VPC Flow Logs – Architectures

Top-10 IP addresses

CloudWatch VPC Flow Logs CloudWatch Logs

Contributor Insights

Alert

Metric Filter

SSH, RDP…

VPC Flow Logs CloudWatch Logs CW Alarm Amazon SNS

Amazon Amazon

VPC Flow Logs S3 Bucket

Athena QuickSight


### VPC Flow Logs – CloudWatch Permissions

- IAM Service Role associated with VPC Flow Logs must have the required

permissions to publish logs to CloudWatch Logs

- logs:CreateLogGroup, logs:CreateLogStream, or logs:PutLogEvents

IAM Service Role

logs

VPC Flow Logs CloudWatch Logs


### AWS Site-to-Site VPN

VPC Flow Logs

Region

NACL NACL

Internet

Corporate

www Public Subnet Private Subnet Data Center

CloudWatch

Security Group

Internet Router

Gateway

NAT Gateway

Amazon

DynamoDB Private EC2 Instance S3

Route Route Server

Customer

Table Security Group Table Security Group VPC Gateway

Endpoints

S2S VPN

Connection

VPC Peering Public EC2 Instance Private EC2 Instance

Connections VPN

Gateway

Availability Zone


|  |  |  |
| --- | --- | --- |
| VPC rnet Router way | NACL NACL Public Subnet Private Subnet Security Group NAT Gateway Private EC2 Instance Route Route Table Security Group Table Security Group E Public EC2 Instance Private EC2 Instance | VPC ndpoint VP Gate |
|  | Availability Zone |  |


### AWS Site-to-Site VPN

- Virtual Private Gateway (VGW)
- VPN concentrator on the AWS side of the VPN connection
- VGW is created and attached to the VPC from which you want to create the

Site-to-Site VPN connection

- Possibility to customize the ASN (Autonomous System Number)
- Customer Gateway (CGW)
- Software application or physical device on customer side of the VPN connection
- https://docs.aws.amazon.com/vpn/latest/s2svpn/your-cgw.html#DevicesTested

### Site-to-Site VPN Connections

Route Table

(Route Propagation enabled)

Private Subnet

- Customer Gateway Device (On-premises)

Security Group

- What IP address to use?
- Public Internet-routable IP address for your Customer

Gateway device

Virtual Private

- If it’s behind a NAT device that’s enabled for NAT

Gateway

traversal (NAT-T), use the public IP address of the NAT

device

- Important step: enable Route Propagation for

OR Customer

the Virtual Private Gateway in the route table

Gateway

that is associated with your subnets (Public IP)

NAT Device

- If you need to ping your EC2 instances from (Public IP)

Corporate Data Center

on-premises, make sure you add the ICMP

protocol on the inbound of your security

groups Customer

Gateway

(Private IP)

Server


| NAT Device (Public IP) |
| --- |
|  |


### AWS VPN CloudHub

- Provide secure communication between

multiple sites, if you have multiple VPN VPC Customer Network

connections

Availability Zone

- Low-cost hub-and-spoke model for Customer

Private Subnet 1

primary or secondary network connectivity Gateway

between different locations (VPN only)

Customer Network

EC2 Instances

- It’s a VPN connection so it goes over the

public Internet

Availability Zone

Virtual Customer

Gateway

Private

- To set it up, connect multiple VPN Private Subnet 2

Gateway

connections on the same VGW, setup

(VGW)

dynamic routing and configure route tables Customer Network

EC2 Instances

Customer

Gateway


| Customer Network ustomer | .datacumulu |
| --- | --- |


### Direct Connect (DX)

- Provides a dedicated private connection from a remote network to your VPC
- Dedicated connection must be setup between your DC and AWS Direct

Connect locations

- You need to setup a Virtual Private Gateway on your VPC
- Access public resources (S3) and private (EC2) on same connection
- Use Cases:
- Increase bandwidth throughput - working with large data sets – lower cost
- More consistent network experience - applications using real-time data feeds
- Hybrid Environments (on prem + cloud)
- Supports both IPv4 and IPv6

### Direct Connect Diagram

Region

(us-east-1)

VPC Corporate

data center

Private Subnet

VLAN 1

VLAN 2

Virtual Private Gateway AWS Direct Customer or Customer

Connect Endpoint partner router router/firewall

EC2 Instances

Customer or

AWS Cage partner cage

AWS Direct Connect Location Customer Network

Amazon Glacier Amazon S3

Private virtual interface

Public virtual interface


### Direct Connect Gateway

- If you want to setup a Direct Connect to one or more VPC in many

different regions (same account), you must use a Direct Connect Gateway

Region Region

(us-east-1) (us-west-1)

VPC VPC

Customer network

10.0.0.0/16 172.16.0.0/16

Private virtual

Private virtual

interface

interface

AWS Direct

Private virtual

interface Connect

connection

Direct Connect Gateway


### Direct Connect – Connection Types

- Dedicated Connections: 1Gbps,10 Gbps and 100 Gbps capacity
- Physical ethernet port dedicated to a customer
- Request made to AWS first, then completed by AWS Direct Connect Partners
- Hosted Connections: 50Mbps, 500 Mbps, to 10 Gbps
- Connection requests are made via AWS Direct Connect Partners
- Capacity can be added or removed on demand
- 1, 2, 5, 10 Gbps available at select AWS Direct Connect Partners
- Lead times are often longer than 1 month to establish a new connection

### Direct Connect – Encr yption

Region

- Data in transit is not encrypted but is

(us-east-1)

private

Availability Zone Corporate

(us-east-1a)

- AWS Direct Connect + VPN data center

Private Subnet 1

provides an IPsec-encrypted private

Client Client

connection

EC2 Instances

Availability Zone

- Good for an extra level of security,

(us-east-1b) AWS Direct VPN Customer

Connect Endpoint Connection router/firewall

but slightly more complex to put in

Private Subnet 2

place

AWS Direct

Connect Location Customer Network

EC2 Instances


### Direct Connect - Resiliency

High Resiliency for Critical Workloads Maximum Resiliency for Critical Workloads

Corporate Corporate

data center data center

Region Region

AWS Direct

Connect Location - 1 AWS Direct

Connect Location - 1

Corporate

Corporate

data center

data center

AWS Direct

Connect Location - 2

AWS Direct

Connect Location - 2

One connection at multiple locations Maximum resilience is achieved by separate connections

terminating on separate devices in more than one location.


### Site-to-Site VPN connection as a backup

- In case Direct Connect fails, you can set up a backup Direct Connect

connection (expensive), or a Site-to-Site VPN connection

AWS Cloud

Direct Connect

Primary Connection

Site-to-Site VPN

Corporate DC

Backup Connection


### Network topologies can become complicated

VPC Peering

VPN Connection Connection

Customer Gateway Amazon VPC Amazon VPC

VPC Peering

Connection

VPN Connection

VPC Peering VPC Peering

Connection Connection

Direct Connect

Gateway

VPN Connection VPC Peering

Amazon VPC Amazon VPC

Connection


### Transit Gateway

- For having transitive peering between thousands of VPC and

on-premises, hub-and-spoke (star) connection

AWS Direct

Connect Gateway

- Regional resource, can work cross-region
- Share cross-account using Resource Access Manager (RAM)
- You can peer Transit Gateways across regions

Amazon VPC Amazon VPC

- Route Tables: limit which VPC can talk with other VPC
- Works with Direct Connect Gateway, VPN connections

Transit

- Supports IP Multicast (not supported by any other AWS

Gateway

service)

Amazon VPC Amazon VPC

VPN Connection

Customer Gateway


### Transit Gateway: Site-to-Site VPN ECMP

- ECMP = Equal-cost multi-path

routing

- Routing strategy to allow to V

forward a packet over multiple c h m Corporate data center

best path

VPC VPC

attachm

VPN attachment 172.16.0.0/16

- Use case: create multiple Site-

attach m

to-Site VPN connections to m

AWS Transit Gateway

increase the bandwidth of your C

connection to AWS


### Transit Gateway: throughput with ECMP

VPN to transit gateway

VPN to virtual private gateway

VPC VPC

1x = 1x 1x = 1x VPC

1x = 1.25 Gbps 1x = 2.5 Gbps (ECMP) – 2 tunnels used

2x = 5.0 Gbps (ECMP)

3x = 7.5 Gbps (ECMP)

VPN connection

(2 tunnels)

per GB of TGW

processed data


| VPC VPC VPC VP |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
|  | VPC VPC VPC VP |  |  |  |  |
|  |  | VPC VPC VP |  |  |  |
|  |  |  | VPC VP |  |  |
|  |  |  |  |  |  |
|  |  |  |  |  |  |


### Transit Gateway – Share Direct Connect

between multiple accounts

AWS Cloud

Corporate

Region

data center

Account 1

Clients Clients

Transit VIF VLAN

Transit Direct AWS Direct Customer

Gateway Connect Connect endpoint router/firewall

Account 2

Gateway

AWS Direct Servers

Connect Location

You can use AWS Resource Access Manager to share Transit

Gateway with other accounts.


### VPC – Traffic Mirroring

Source A Source B

- Allows you to capture and inspect network

traffic in your VPC

- Route the traffic to security appliances that

you manage Inbound & Inbound &

Outbound traffic Outbound

- Capture the traffic

traffic

- From (Source) – ENIs
- To (Targets) – an ENI or a Network Load Balancer

Traffic Mirroring

- Capture all packets or capture the packets of

(filter traffic, optional)

your interest (optionally, truncate packets)

Network Load

- Source and Target can be in the same VPC or

Balancer

different VPCs (VPC Peering)

- Use cases: content inspection, threat

Auto Scaling group

monitoring, troubleshooting, …

EC2 instances with Security Appliances


### What is IPv6?

- IPv4 designed to provide 4.3 Billion addresses (they’ll be exhausted soon)
- IPv6 is the successor of IPv4
- IPv6 is designed to provide 3.4 × 10 unique IP addresses
- Every IPv6 address in AWS is public and Internet-routable (no private range)
- Format è x.x.x.x.x.x.x.x (x is hexadecimal, range can be from 0000 to ffff)
- Examples:
- 2001:db8:3333:4444:5555:6666:7777:8888
- 2001:db8:3333:4444:cccc:dddd:eeee:ffff
- :: è all 8 segments are zero
- 2001:db8:: è the last 6 segments are zero
- ::1234:5678 è the first 6 segments are zero
- 2001:db8::1234:5678 è the middle 4 segments are zero

### IPv6 in VPC

Internet

- IPv4 cannot be disabled for your VPC and

subnets

- You can enable IPv6 (they’re public IP addresses)

to operate in dual-stack mode

Internet

Gateway

IPv4 & IPv6

- Your EC2 instances will get at least a private

internal IPv4 and a public IPv6

- They can communicate using either IPv4 or IPv6

EC2 Instance

to the internet through an Internet Gateway

(Private IP: 10.0.0.5)

(IPv6: 2001:db8::ff00:42:8329)


### IPv4 Troubleshooting

User

- IPv4 cannot be disabled for your VPC

and subnets

- So, if you cannot launch an EC2 instance create

in your subnet

- It’s not because it cannot acquire an IPv6

(IPv4: 192.168.0.0/24)

(the space is very large)

(IPv4: 10.0.0.0/24)

(IPv6: 2001:db8:1234:5678::/56)

- It’s because there are no available IPv4 in

your subnet …

192.168.0.10 192.168.0.15

- Solution: create a new IPv4 CIDR in

your subnet

10.0.0.35


### Egress-only Internet Gateway

- Used for IPv6 only

Internet

- (similar to a NAT Gateway but for IPv6)

can’t initiate

initiate connections

connections from

from both sides

Internet

- Allows instances in your VPC outbound

connections over IPv6 while preventing

the internet to initiate an IPv6 connection

Internet Egress-only

Gateway Internet Gateway

to your instances

- You must update the Route Tables

Public Subnet Private Subnet

IPv6: 2001:db8::b1c2 IPv6: 2001:db8::e1c3


### IPv6 Routing

Route Table

(Public Subnet)

Region

Destination Target

10.0.0.0/16 local

VPC 2001:db8:1234:1a00::/56 local

NAT Gateway

(IPv4: 10.0.0.0/16)

0.0.0.0/0 igw-id

(IPv6: 2001:db8:1234:1a00::/56) (IPv4)

::/0 igw-id

Public Subnet

(IPv4: 10.0.0.0/24) EIP: 198.51.100.1

(IPv6: 2001:db8:1234:1a00::/64)

Internet

Private IPv4: 10.0.0.5

EIP: 198.51.100.1 Gateway Internet

IPv6: 2001:db8:1234:1a00::123 Web server (IPv4 & IPv6)

Private Subnet

Route Table

(IPv4: 10.0.1.0/24)

(IPv6: 2001:db8:1234:1a02::/64) Egress-only (Private Subnet)

Internet Gateway Destination Target

Private IPv4: 10.0.1.5

(IPv6)

IPv6: 2001:db8:1234:1a02::456

10.0.0.0/16 local

Server

2001:db8:1234:1a00::/56 local

0.0.0.0/0 nat-gateway-id

::/0 eigw-id


| Destination | Target |
| --- | --- |
| 10.0.0.0/16 | local |
| 2001:db8:1234:1a00::/56 | local |
| 0.0.0.0/0 | igw-id |
| ::/0 | igw-id |


| Destination | Target |
| --- | --- |
| 10.0.0.0/16 | local |
| 2001:db8:1234:1a00::/56 | local |
| 0.0.0.0/0 | nat-gateway-id |
| ::/0 | eigw-id |


### VPC Section Summary (1/3)

- CIDR – IP Range
- VPC – Virtual Private Cloud => we define a list of IPv4 & IPv6 CIDR
- Subnets – tied to an AZ, we define a CIDR
- Internet Gateway – at the VPC level, provide IPv4 & IPv6 Internet Access
- Route Tables – must be edited to add routes from subnets to the IGW, VPC Peering

Connections, VPC Endpoints, …

- Bastion Host – public EC2 instance to SSH into, that has SSH connectivity to EC2

instances in private subnets

- NAT Instances – gives Internet access to EC2 instances in private subnets. Old, must

be setup in a public subnet, disable Source / Destination check flag

- NAT Gateway – managed by AWS, provides scalable Internet access to private EC2

instances, when the target is an IPv4 address


### VPC Section Summary (2/3)

- NACL – stateless, subnet rules for inbound and outbound, don’t forget Ephemeral

Ports

- Security Groups – stateful, operate at the EC2 instance level
- VPC Peering – connect two VPCs with non overlapping CIDR, non-transitive
- VPC Endpoints – provide private access to AWS Services (S3, DynamoDB,

CloudFormation, SSM) within a VPC

- VPC Flow Logs – can be setup at the VPC / Subnet / ENI Level, for ACCEPT and

REJECT traffic, helps identifying attacks, analyze using Athena or CloudWatch Logs

Insights

- Site-to-Site VPN – setup a Customer Gateway on DC, a Virtual Private Gateway on

VPC, and site-to-site VPN over public Internet

- AWS VPN CloudHub – hub-and-spoke VPN model to connect your sites

### VPC Section Summary (3/3)

- Direct Connect – setup a Virtual Private Gateway on VPC, and establish a

direct private connection to an AWS Direct Connect Location

- Direct Connect Gateway – setup a Direct Connect to many VPCs in different

AWS regions

- AWS PrivateLink / VPC Endpoint Services:
- Connect services privately from your service VPC to customers VPC
- Doesn’t need VPC Peering, public Internet, NAT Gateway, Route Tables
- Must be used with Network Load Balancer & ENI
- ClassicLink – connect EC2-Classic EC2 instances privately to your VPC
- Transit Gateway – transitive peering connections for VPC, VPN & DX
- Traffic Mirroring – copy network traffic from ENIs for further analysis
- Egress-only Internet Gateway – like a NAT Gateway, but for IPv6 targets

### Networking Costs in AWS per GB - Simplified

Region Region

- Use Private IP

Availability Zone Availability Zone Availability Zone

instead of Public

IP for good

Free for traffic in

savings and

Free if using better network

private IP

performance

$0.01 if $0.02

Using private IP Inter-region

- Use same AZ for

maximum savings

(at the cost of

$0.02 if using

high availability)

Public IP / Elastic IP


### Minimizing egress traffic network cost

- Egress traffic: outbound

Egress cost is high

traffic (from AWS to

outside)

AWS Cloud Corporate data center

- Ingress traffic: inbound

DB Query

Query Results

traffic - from outside to

100 MB

50 KB

AWS (typically free)

- Try to keep as much

Application

Database

internet traffic within

AWS to minimize costs

Egress cost is minimized

- Direct Connect location

AWS Cloud Corporate data center

that are co-located in

the same AWS Region

Query Results

DB Query

result in lower cost for

50 KB

100 MB

egress network

Application

Database


### S3 Data Transfer Pricing – Analysis for USA

- S3 ingress: free

internet

- S3 to Internet: $0.09 per GB
- S3 Transfer Acceleration:

$0.09

- Faster transfer times (50 to 500% better)
- Additional cost on top of Data Transfer

Transfer acceleration +$0.04

Pricing: +$0.04 to $0.08 per GB

- S3 to CloudFront: $0.00 per GB

Edge location

- CloudFront to Internet: $0.085 per GB

(slightly cheaper than S3)

$0.00

- Caching capability (lower latency)
- Reduce costs associated with S3 Requests Replication $0.02

$0.085

Pricing (7x cheaper with CloudFront)

- S3 Cross Region Replication: $0.02 per GB

CloudFront


### Pricing:

NAT Gateway vs Gateway VPC Endpoint

Region

(us-east-1)

$0.045 NAT Gateway / hour

$0.045 NAT Gateway data processed / GB

(10.0.0.0/16)

Subnet 1 route table

$0.09 Data transfer out to S3 (cross-region)

Destination Target Private subnet 1 Public subnet

$0.00 Data transfer out to S3 (same-region)

(10.0.0.0/24)

10.0.0.0/16 Local

0.0.0.0/0 igw-id

Internet

EC2 Instance NAT Gateway

Internet

Subnet 2 route table Gateway

Destination Target Private subnet 2

(10.0.1.0/24)

10.0.0.0/16 Local

pl-id for vpce-id

No cost for using Gateway Endpoint.

Amazon S3

$0.01 Data transfer in/out (same-

EC2 Instance VPC Endpoint S3 Bucket region)


| Destination | Target |
| --- | --- |
| 10.0.0.0/16 | Local |
| 0.0.0.0/0 | igw-id |


| Destination | Target |
| --- | --- |
| 10.0.0.0/16 | Local |
| pl-id for Amazon S3 | vpce-id |


### Network Protection on AWS

- To protect network on AWS, we’ve seen
- Network Access Control Lists (NACLs)
- Amazon VPC security groups
- AWS WAF (protect against malicious requests)
- AWS Shield & AWS Shield Advanced
- AWS Firewall Manager (to manage them across accounts)
- But what if we want to protect in a sophisticated way our entire VPC?

### AWS Network Firewall

internet

- Protect your entire Amazon VPC

AWS Network Firewall

- From Layer 3 to Layer 7 protection
- Any direction, you can inspect
- VPC to VPC traffic VPC

Direct Connect

- Outbound to internet
- Inbound from internet

Private subnet

- To / from Direct Connect & Site-to-Site VPN

Corporate DC

- Internally, the AWS Network Firewall uses

VPN connection

the AWS Gateway Load Balancer

- Rules can be centrally managed cross-

account by AWS Firewall Manager to apply

to many VPCs

Peered VPC


### Network Firewall – Fine Grained Controls

- Supports 1000s of rules
- IP & port - example: 10,000s of IPs filtering
- Protocol – example: block the SMB protocol for outbound communications
- Stateful domain list rule groups: only allow outbound traffic to *.mycorp.com or third-party

software repo

- General pattern matching using regex
- Traffic filtering: Allow, drop, or alert for the traffic that matches the rules
- Active flow inspection to protect against network threats with intrusion-prevention

capabilities (like Gateway Load Balancer, but all managed by AWS)

- Send logs of rule matches to Amazon S3, CloudWatch Logs, Kinesis Data Firehose

---

## Disaster Recover y & Migrations


### Disaster Recover y Over view

- Any event that has a negative impact on a company’s business continuity

or finances is a disaster

- Disaster recovery (DR) is about preparing for and recovering from a

disaster

- What kind of disaster recovery?
- On-premise => On-premise: traditional DR, and very expensive
- On-premise => AWS Cloud: hybrid recovery
- AWS Cloud Region A => AWS Cloud Region B
- Need to define two terms:
- RPO: Recovery Point Objective
- RTO: Recovery Time Objective

### RPO and RTO

Data loss Downtime

RPO RTO

Disaster


### Disaster Recover y Strategies

- Backup and Restore
- Pilot Light
- Warm Standby
- Hot Site / Multi Site Approach

Faster RTO


### Backup and Restore (High RPO)

Corporate data AWS Cloud

center

AWS Storage Gateway

Amazon S3

AWS Snowball

Glacier

AWS Cloud

Redshift

Snapshot

lifecycle

AWS Cloud

Amazon EC2

Scheduled regular

snapshots

Amazon RDS


### Disaster Recover y – Pilot Light

- A small version of the app is always running in the cloud
- Useful for the critical core (pilot light)
- Very similar to Backup and Restore
- Faster than Backup and Restore as critical systems are already up

AWS Cloud

Corporate data

Route 53

center

EC2 (not running)

Data Replication

RDS (running)


### Warm Standby

- Full system is up and running, but at minimum size
- Upon disaster, we can scale to production load

Corporate data AWS Cloud

center

Route 53

Reverse

proxy

Server

EC2 Auto Scaling failover

(minimum)

Data Replication

Primary

RDS Secondary (running)


### Multi Site / Hot Site Approach

- Very low RTO (minutes or seconds) – very expensive
- Full Production Scale is running AWS and On Premise

Corporate data AWS Cloud

active active

center

Route 53

Reverse

proxy

Server

EC2 Auto Scaling failover

(production)

Data Replication

Primary

RDS secondary (running)


### All AWS Multi Region

AWS Cloud AWS Cloud

active active

Route 53

ELB ELB

EC2 Auto Scaling failover

EC2 Auto Scaling

(production)

(production)

Data Replication

Aurora Global (secondary)

Aurora Global (primary)


### Disaster Recover y Tips

- Backup
- EBS Snapshots, RDS automated backups / Snapshots, etc…
- Regular pushes to S3 / S3 IA / Glacier, Lifecycle Policy, Cross Region Replication
- From On-Premise: Snowball or Storage Gateway
- High Availability
- Use Route53 to migrate DNS over from Region to Region
- RDS Multi-AZ, ElastiCache Multi-AZ, EFS, S3
- Site to Site VPN as a recovery from Direct Connect
- Replication
- RDS Replication (Cross Region), AWS Aurora + Global Databases
- Database replication from on-premises to RDS
- Storage Gateway
- Automation
- CloudFormation / Elastic Beanstalk to re-create a whole new environment
- Recover / Reboot EC2 instances with CloudWatch if alarms fail
- AWS Lambda functions for customized automations
- Chaos
- Netflix has a “simian-army” randomly terminating EC2

### AWS Elastic Disaster Recover y (DRS)

- Used to be named “CloudEndure Disaster Recovery”
- Quickly and easily recover your physical, virtual, and cloud-based servers into AWS
- Example: protect your most critical databases (including Oracle, MySQL, and SQL Server),

enterprise apps (SAP), protect your data from ransomware attacks, …

- Continuous block-level replication for your servers

Corporate Data Center / Any cloud AWS Cloud

Elastic Disaster Recovery

Staging Production

continuous replication

Apps

(seconds)

failover

AWS Replication (minutes)

Agent

Disks Low-cost EC2 instances Target EC2 instances

& EBS volumes & EBS volumes

failback


### DMS – Database Migration Ser vice

- Quickly and securely migrate databases to

AWS, resilient, self healing

Source DB

- The source database remains available

during the migration

- Supports:

EC2 instance

- Homogeneous migrations: ex Oracle to

Running DMS

Oracle

- Heterogeneous migrations: ex Microsoft SQL

Server to Aurora

- Continuous Data Replication using CDC

Target DB

- You must create an EC2 instance to

perform the replication tasks


### DMS Sources and Targets

SOURCES: TARGETS:

- On-Premises and EC2 instances
- On-Premises and EC2 instances

databases: Oracle, MS SQL Server,

databases: Oracle, MS SQL Server,

MySQL, MariaDB, PostgreSQL, SAP

MySQL, MariaDB, PostgreSQL,

- Amazon RDS

MongoDB, SAP, DB2

- Redshift, DynamoDB, S3
- Azure: Azure SQL Database
- OpenSearch Service
- Amazon RDS: all including
- Kinesis Data Streams

Aurora

- Apache Kafka
- Amazon S3
- DocumentDB & Amazon Neptune
- DocumentDB
- Redis & Babelfish

### AWS Schema Conversion Tool (SCT)

- Convert your Database’s Schema from one engine to another
- Example OLTP: (SQL Server or Oracle) to MySQL, PostgreSQL, Aurora
- Example OLAP: (Teradata or Oracle) to Amazon Redshift
- Prefer compute-intensive instances to optimize data conversions

Target DB (different engine)

Source DB DMS + SCT

- You do not need to use SCT if you are migrating the same DB engine
- Ex: On-Premise PostgreSQL => RDS PostgreSQL
- The DB engine is still PostgreSQL (RDS is the platform)

### DMS - Continuous Replication

Region

Corporate data center

Public Subnet Private Subnet

Data migration

Full load +

Oracle DB Amazon RDS

(source) for MySQL DB

AWS DMS

(target)

Replication

Instance

Schema conversion

Server with

AWS SCT installed


### AWS DMS – Multi-AZ Deployment

- When Multi-AZ Enabled, DMS

provisions and maintains a

AWS Region

synchronously stand replica in a

Availability Zone - A Availability Zone - B

different AZ

- Advantages: synchronous

replication

- Provides Data Redundancy

DMS Replication DMS Replication

- Eliminates I/O freezes

Instance Instance

- Minimizes latency spikes (Standby Replica)

### RDS & Aurora MySQL Migrations

- RDS MySQL to Aurora MySQL
- Option 1: DB Snapshots from RDS MySQL restored as

MySQL Aurora DB

- Option 2: Create an Aurora Read Replica from your RDS

Read Replica

MySQL, and when the replication lag is 0, promote it as its

own DB cluster (can take time and cost $)

- External MySQL to Aurora MySQL

Percona

- Option 1:

XtraBackup import

- Use Percona XtraBackup to create a file backup in Amazon S3
- Create an Aurora MySQL DB from Amazon S3
- Option 2:
- Create an Aurora MySQL DB
- Use the mysqldump utility to migrate MySQL into Aurora

mysqldump

(slower than S3 method)

- Use DMS if both databases are up and running

### RDS & Aurora PostgreSQL Migrations

- RDS PostgreSQL to Aurora PostgreSQL
- Option 1: DB Snapshots from RDS PostgreSQL restored

as PostgreSQL Aurora DB

- Option 2: Create an Aurora Read Replica from your RDS Read Replica

PostgreSQL, and when the replication lag is 0, promote it

as its own DB cluster (can take time and cost $)

- External PostgreSQL to Aurora PostgreSQL backup import
- Create a backup and put it in Amazon S3
- Import it using the aws_s3 Aurora extension
- Use DMS if both databases are up and running

### On-Premise strategy with AWS

- Ability to download Amazon Linux 2 AMI as a VM (.iso format)
- VMWare, KVM, VirtualBox (Oracle VM), Microsoft Hyper-V
- VM Import / Export
- Migrate existing applications into EC2
- Create a DR repository strategy for your on-premises VMs
- Can export back the VMs from EC2 to on-premises
- AWS Application Discovery Service
- Gather information about your on-premises servers to plan a migration
- Server utilization and dependency mappings
- Track with AWS Migration Hub
- AWS Database Migration Service (DMS)
- replicate On-premise => AWS , AWS => AWS, AWS => On-premise
- Works with various database technologies (Oracle, MySQL, DynamoDB, etc..)
- AWS Application Migration Service (MGN)
- Incremental replication of on-premises live servers to AWS

### AWS Backup

- Fully managed service
- Centrally manage and automate backups across AWS services
- No need to create custom scripts and manual processes
- Supported services:
- Amazon EC2 / Amazon EBS
- Amazon S3
- Amazon RDS (all DBs engines) / Amazon Aurora / Amazon DynamoDB
- Amazon DocumentDB / Amazon Neptune
- Amazon EFS / Amazon FSx (Lustre & Windows File Server)
- AWS Storage Gateway (Volume Gateway)
- Supports cross-region backups
- Supports cross-account backups

### AWS Backup

- Supports PITR for supported services
- On-Demand and Scheduled backups
- Tag-based backup policies
- You create backup policies known as Backup Plans
- Backup frequency (every 12 hours, daily, weekly, monthly, cron expression)
- Backup window
- Transition to Cold Storage (Never, Days, Weeks, Months, Years)
- Retention Period (Always, Days, Weeks, Months, Years)

### AWS Backup

Assign AWS Resources

EC2 EBS S3

Create Backup Plan

RDS DynamoDB DocumentDB

Automatically

(frequency, retention

backed up to

policy)

AWS Backup Amazon S3

EFS Aurora Neptune

FSx Storage

Gateway


### AWS Backup Vault Lock

- Enforce a WORM (Write Once Read Many)

state for all the backups that you store in

your AWS Backup Vault

backup

- Additional layer of defense to protect your

backups against:

- Inadvertent or malicious delete operations
- Updates that shorten or alter retention periods

Backup Vault Lock Policy

Backups can’t be deleted

- Even the root user cannot delete backups

when enabled


### AWS Application Discover y Ser vice

- Plan migration projects by gathering information about on-premises data centers
- Server utilization data and dependency mapping are important for migrations
- Agentless Discovery (AWS Agentless Discovery Connector)
- VM inventory, configuration, and performance history such as CPU, memory, and disk usage
- Agent-based Discovery (AWS Application Discovery Agent)
- System configuration, system performance, running processes, and details of the network

connections between systems

- Resulting data can be viewed within AWS Migration Hub

### AWS Application Migration Ser vice (MGN)

- The “AWS evolution” of CloudEndure Migration, replacing AWS Server Migration Service (SMS)
- Lift-and-shift (rehost) solution which simplify migrating applications to AWS
- Converts your physical, virtual, and cloud-based servers to run natively on AWS
- Supports wide range of platforms, Operating Systems, and databases
- Minimal downtime, reduced costs

Corporate Data Center / Any cloud AWS Cloud

Application Migration Service

Staging Production

continuous replication

Apps

cutover

AWS Replication

Agent

Disks Low-cost EC2 instances Target EC2 instances

& EBS volumes & EBS volumes


### VMware Cloud on AWS

- Some customers use VMware Cloud to manage their on-premises Data Center
- They want to extend the Data Center capacity to AWS, but keep using the VMware Cloud software
- …Enter VMware Cloud on AWS
- Use cases
- Migrate your VMware vSphere-based workloads to AWS
- Run your production workloads across VMware vSphere-based private, public, and hybrid cloud environments
- Have a disaster recover strategy

Customer Data Center AWS Cloud

AWS Services

Amazon Amazon Direct

EC2 S3 Connect

VMware Cloud

on AWS

vSphere

On-Premises vCenter

Amazon Amazon Amazon

vSphere-based

FSx RDS Redshift

environment


### Transferring large amount of data into AWS

- Example: transfer 200 TB of data in the cloud. We have a 100 Mbps internet

connection.

- Over the internet / Site-to-Site VPN:
- Immediate to setup
- Will take 200(TB)*1000(GB)*1000(MB)*8(Mb)/100 Mbps = 16,000,000s = 185d
- Over direct connect 1Gbps:
- Long for the one-time setup (over a month)
- Will take 200(TB)*1000(GB)*8(Gb)/1 Gbps = 1,600,000s = 18.5d
- Over Snowball:
- Takes about 1 week for the end-to-end transfer
- Can be combined with DMS
- For on-going replication / transfers: Site-to-Site VPN or DX with DMS or

DataSync


---

## More Solutions Architecture


### Lambda, SNS & SQS

Try, retry

retries

(poll)

SQS asynchronous

DLQ SQS + Lambda

Try, retry

blocking

SQS FIFO

SNS + Lambda

SQS FIFO + Lambda


### Fan Out Pattern: deliver to multiple SQS

SQS SQS

PUT #1

SDK SDK

PUT #2 PUT subscribe

PUT #3

Option 1 Option 2 – Fan Out


### S3 Event Notifications

- S3:ObjectCreated, S3:ObjectRemoved,

S3:ObjectRestore, S3:Replication…

- Object name filtering possible (*.jpg) SNS
- Use case: generate thumbnails of images

uploaded to S3

events

- Can create as many “S3 events” as desired

Amazon S3

- S3 event notifications typically deliver events

in seconds but can sometimes take a minute

or longer

Lambda Function


### S3 Event Notifications

with Amazon EventBridge

events All events rules

Over 18

AWS services

as destinations

Amazon S3 Amazon

bucket EventBridge

- Advanced filtering options with JSON rules (metadata, object size, name...)
- Multiple Destinations – ex Step Functions, Kinesis Streams / Firehose…
- EventBridge Capabilities – Archive, Replay Events, Reliable delivery

### Amazon EventBridge – Intercept API Calls

User

DeleteTable API Call 💥

alert

Log API call event

DynamoDB Amazon

CloudTrail SNS

EventBridge

(any API call)


### API Gateway – AWS Ser vice Integration

Kinesis Data Streams example

store .json

requests send records files

API Gateway Kinesis Data Kinesis Data Amazon S3

Client

Streams Firehose


### Caching Strategies

Redis

Memcached

CloudFront API Gateway App logic

Client

EC2 / Lambda

Database

CloudFront (edge)

Caching, TTL, Network, Computation, Cost, Latency


### Blocking an IP address

Public Subnet

Security Group (allow rules)

NACL

Client EC2 Instance

Deny + Allow rules

public IP + Firewall Software (optional)


### Blocking an IP address – with an ALB

Public Subnet Private Subnet

ALB Security Group EC2 Security Group

Client NACL Application Load Balancer EC2 Instance

Connection Termination Private IP


### Blocking an IP address – with an NLB

Public Subnet Private Subnet

NLB Security Group EC2 Security Group

NACL

Client Network Load Balancer EC2 Instance

Private IP


### Blocking an IP address – ALB + WAF

Public Subnet Private Subnet

ALB Security Group EC2 Security Group

NACL

Client Application Load Balancer EC2 Instance

Private IP

IP Address Filtering

AWS WAF


### Blocking an IP address – ALB, CloudFront & WAF

Public Subnet Private Subnet

ALB Security Group EC2 Security Group

CloudFront

Public IPs

NACL

Client Application Load Balancer EC2 Instance

CloudFront

NOT helpful Public Private IP

(Geo Restriction)

AWS WAF

(IP Address Filtering)


### High Performance Computing (HPC)

- The cloud is the perfect place to perform HPC
- You can create a very high number of resources in no time
- You can speed up time to results by adding more resources
- You can pay only for the systems you have used
- Perform genomics, computational chemistry, financial risk modeling,

weather prediction, machine learning, deep learning, autonomous driving

- Which services help perform HPC?

### Data Management & Transfer

- AWS Direct Connect:
- Move GB/s of data to the cloud, over a private secure network
- Snowball & Snowmobile
- Move PB of data to the cloud
- AWS DataSync
- Move large amount of data between on-premises and S3, EFS, FSx for Windows

### Compute and Networking

- EC2 Instances:
- CPU optimized, GPU optimized
- Spot Instances / Spot Fleets for cost savings + Auto Scaling
- EC2 Placement Groups: Cluster for good network performance

EC2 EC2 EC2

Placement group

Cluster

Same Rack

Same AZ Low latency

10Gbps network

EC2 EC2 EC2


| EC2 |  |
| --- | --- |
|  |  |
| EC2 |  |


| EC2 |  |
| --- | --- |
|  |  |
| EC2 |  |


### Compute and Networking

- EC2 Enhanced Networking (SR-IOV)
- Higher bandwidth, higher PPS (packet per second), lower latency
- Option 1: Elastic Network Adapter (ENA) up to 100 Gbps
- Option 2: Intel 82599 VF up to 10 Gbps – LEGACY
- Elastic Fabric Adapter (EFA)
- Improved ENA for HPC, only works for Linux
- Great for inter-node communications, tightly coupled workloads
- Leverages Message Passing Interface (MPI) standard
- Bypasses the underlying Linux OS to provide low-latency, reliable transport

### Storage

- Instance-attached storage:
- EBS: scale up to 256,000 IOPS with io2 Block Express
- Instance Store: scale to millions of IOPS, linked to EC2 instance, low latency
- Network storage:
- Amazon S3: large blob, not a file system
- Amazon EFS: scale IOPS based on total size, or use provisioned IOPS
- Amazon FSx for Lustre:
- HPC optimized distributed file system, millions of IOPS
- Backed by S3

### Automation and Orchestration

- AWS Batch
- AWS Batch supports multi-node parallel jobs, which enables you to run single

jobs that span multiple EC2 instances.

- Easily schedule jobs and launch EC2 instances accordingly
- AWS ParallelCluster
- Open-source cluster management tool to deploy HPC on AWS
- Configure with text files
- Automate creation of VPC, Subnet, cluster type and instance types
- Ability to enable EFA on the cluster (improves network performance)

### Creating a highly available EC2 instance

monitor

Attachment

CloudWatch Event

What time is it?

Public EC2

(or Alarm based on metric)

5:30 pm!

Elastic IP Address

Start the instance

Attach the Elastic IP

Standby EC2 instance


### Creating a highly available EC2 instance

With an Auto Scaling Group

ASG Settings

Auto Scaling group

1 min

Availability Zone 1

1 max

1 desired

>= 2 AZ

What time is it?

EC2 user data to attach

Public EC2 The Elastic IP

5:30 pm!

Elastic IP Address EC2 instance role to

Availability Zone 2

Allow API calls to attach

The Elastic IP

EC2 User Data

Attachment

Based on Tag

Replacement

EC2 instance


### Creating a highly available EC2 instance

With ASG + EBS

Auto Scaling group

Availability Zone 1

EBS Snapshot

On ASG Terminate lifecycle hook

What time is it?

EBS Volume

Public EC2

5:30 pm!

Elastic IP Address EBS Snapshot

Availability Zone 2

+ tags

EC2 User Data

Attachment

Based on Tag

EBS Volume created + attached

On ASG Launch lifecycle hook

Replacement

EC2 instance


### Other Ser vices

Overview of Services that might come up in a few questions


### What is CloudFormation

- CloudFormation is a declarative way of outlining your AWS

Infrastructure, for any resources (most of them are supported).

- For example, within a CloudFormation template, you say:
- I want a security group
- I want two EC2 instances using this security group
- I want an S3 bucket
- I want a load balancer (ELB) in front of these machines
- Then CloudFormation creates those for you, in the right order, with the

exact configuration that you specify


### Benefits of AWS CloudFormation (1/2)

- Infrastructure as code
- No resources are manually created, which is excellent for control
- Changes to the infrastructure are reviewed through code
- Cost
- Each resources within the stack is tagged with an identifier so you can easily see how

much a stack costs you

- You can estimate the costs of your resources using the CloudFormation template
- Savings strategy: In Dev, you could automation deletion of templates at 5 PM and

recreated at 8 AM, safely


### Benefits of AWS CloudFormation (2/2)

- Productivity
- Ability to destroy and re-create an infrastructure on the cloud on the fly
- Automated generation of Diagram for your templates!
- Declarative programming (no need to figure out ordering and orchestration)
- Don’t re-invent the wheel
- Leverage existing templates on the web!
- Leverage the documentation
- Supports (almost) all AWS resources:
- Everything we’ll see in this course is supported
- You can use “custom resources” for resources that are not supported

### CloudFormation + Infrastructure Composer

- Example: WordPress CloudFormation Stack

CloudFormation Infrastructure

- We can see all the resources

Composer

- We can see the relations between the components

### CloudFormation – Ser vice Role

Permissions

- cloudformation:* User

- iam:PassRole

- IAM role that allows CloudFormation to

create/update/delete stack resources on your

behalf

Template

- Give ability to users to create/update/delete the

stack resources even if they don’t have

Service Role

permissions to work with the resources in the

- s3:*Bucket

stack

- Use cases: CloudFormation
- You want to achieve the least privilege principle
- But you don’t want to give the user all the required

permissions to create the stack resources

- User must have iam:PassRole permissions

S3 bucket

Stack


### Amazon Simple Email Service (Amazon SES)

- Fully managed service to send emails securely, globally and at scale
- Allows inbound/outbound emails

Users

- Reputation dashboard, performance insights, anti-spam feedback
- Provides statistics such as email deliveries, bounces, feedback loop

bulk emails

results, email open

- Supports DomainKeys Identified Mail (DKIM) and Sender Policy

Framework (SPF)

- Flexible IP deployment: shared, dedicated, and customer-owned IPs

Amazon SES

- Send emails using your application using AWS Console, APIs, or SMTP

APIs

or SMTP

- Use cases: transactional, marketing and bulk email communications

Application


### Amazon Pinpoint

- Scalable 2-way (outbound/inbound) marketing

communications service

- Supports email, SMS, push, voice, and in-app messaging

Amazon

- Ability to segment and personalize messages with the

Pinpoint

right content to customers

Customers

- Possibility to receive replies
- Scales to billions of messages per day stream events

(e.g., TEXT_SUCCESS,

- Use cases: run campaigns by sending marketing, bulk,

TEXT_DELIVERED, …)

transactional SMS messages

- Versus Amazon SNS or Amazon SES
- In SNS & SES you managed each message's audience,

content, and delivery schedule

- In Amazon Pinpoint, you create message templates,

delivery schedules, highly-targeted segments, and full

campaigns

SNS Kinesis Data CloudWatch

Firehose Logs


### Systems Manager – SSM Session Manager

- Allows you to start a secure shell on your EC2 and

EC2 Instance

on-premises servers

(SSM Agent)

Execute

- No SSH access, bastion hosts, or SSH keys needed commands
- No port 22 needed (better security)

Session

Manager

- Supports Linux, macOS, and Windows
- Send session log data to S3 or CloudWatch Logs IAM

Permissions

User


### Systems Manager – Run Command

- Execute a document (= script) or just run a

command

- Run command across multiple instances

(using resource groups) Amazon SNS

t EventBridge

- No need for SSH a

trigger

- Command Output can be shown in the AWS

Console, sent to S3 bucket or CloudWatch

Logs Amazon S3 output

- Send notifications to SNS about command

Run Command

status (In progress, Success, Failed, …)

- Integrated with IAM & CloudTrail

CloudWatch

- Can be invoked using EventBridge Logs

EC2 Instances EC2 Instances

(with SSM Agent) (with SSM Agent)


### Systems Manager – Patch Manager

- Automates the process of patching managed

instances

- OS updates, applications updates, security

AWS Console AWS SDK Maintenance

updates Windows

- Supports EC2 instances and on-premises

servers AWS-RunBatchBaseline

- Supports Linux, macOS, and Windows
- Patch on-demand or on a schedule using

Run Command

Maintenance Windows

- Scan instances and generate patch compliance

report (missing patches)

EC2 Instances EC2 Instances

(with SSM Agent) (with SSM Agent)


### Systems Manager – Maintenance Windows

- Defines a schedule for when to perform actions on your instances
- Example: OS patching, updating drivers, installing software, …
- Maintenance Window contains
- Schedule
- Duration
- Set of registered instances
- Set of registered tasks

EC2 Instances

trigger every 24 hour update (with SSM Agent)

Maintenance Windows Run Command

EC2 Instances

(with SSM Agent)


### Systems Manager - Automation

- Simplifies common maintenance and

deployment tasks of EC2 instances and other

AWS resources

AWS Console AWS SDK Maintenance Amazon AWS Config

- Examples: restart instances, create an AMI,

Windows EventBridge Remediation

EBS snapshot

execute automation

- Automation Runbook – SSM Documents to

(AWS-RestartEC2Instance)

define actions preformed on your EC2

instances or AWS resources (pre-defined or

custom)

Runbooks SSM Automation

- Can be triggered using:

(automation documents)

- Manually using AWS Console, AWS CLI or SDK

execute

- Amazon EventBridge
- On a schedule using Maintenance Windows

AWS Resources

- By AWS Config for rules remediations

EC2 Instances

EBS AMI RDS


| AWS Resources … EBS AMI RDS | acumulus.co |
| --- | --- |


### Cost Explorer

- Visualize, understand, and manage your AWS costs and usage over time
- Create custom reports that analyze cost and usage data.
- Analyze your data at a high level: total costs and usage across all accounts
- Or Monthly, hourly, resource level granularity
- Choose an optimal Savings Plan (to lower prices on your bill)
- Forecast usage up to 18 months based on previous usage

---

## Cost Explorer – Monthly Cost by AWS Ser vice


---

## Cost Explorer– Hourly & Resource Level


### Cost Explorer – Savings Plan

Alternative to Reser ved Instances


---

## Cost Explorer – Forecast Usage


### AWS Cost Anomaly Detection

- Continuously monitor your cost and usage using ML to detect unusual spends
- It learns your unique, historic spend patterns to detect one-time cost spike

and/or continuous cost increases (you don’t need to define thresholds)

- Monitor AWS services, member accounts, cost allocation tags, or cost categories
- Sends you the anomaly detection report with root-cause analysis
- Get notified with individual alerts or daily/weekly summary (using SNS)

AWS Cost Anomaly Create Cost Monitor Get Alerted Analyze Root Cause

Detection Identify unusual spend at Receive alerts when Analyze the root cause

reduce cost surprises the granularity level unusual spend is detected behind the anomaly and

with Machine Learning that you specify the impact on your costs


### AWS Outposts

- Hybrid Cloud: businesses that keep an on-

premises infrastructure alongside a cloud

infrastructure

- Therefore, two ways of dealing with IT systems:

AWS Corporate

- One for the AWS cloud (using the AWS console,

Cloud data center

CLI, and AWS APIs)

- One for their on-premises infrastructure

On-prem

servers

- AWS Outposts are “server racks” that offers the

same AWS infrastructure, services, APIs & tools

to build your own applications on-premises just as

Extension of Outposts

in the cloud

AWS services Racks

- AWS will setup and manage “Outposts Racks”

within your on-premises infrastructure and you

can start leveraging AWS services on-premises

- You are responsible for the Outposts Rack

physical security


### AWS Outposts

- Benefits:
- Low-latency access to on-premises systems
- Local data processing
- Data residency
- Easier migration from on-premises to the cloud
- Fully managed service
- Some services that work on Outposts:

Amazon EC2 Amazon EBS Amazon S3 Amazon EKS Amazon ECS Amazon RDS Amazon EMR


### AWS Batch

- Fully managed batch processing at any scale
- Efficiently run 100,000s of computing batch jobs on AWS
- A “batch” job is a job with a start and an end (opposed to continuous)
- Batch will dynamically launch EC2 instances or Spot Instances
- AWS Batch provisions the right amount of compute / memory
- You submit or schedule batch jobs and AWS Batch does the rest!
- Batch jobs are defined as Docker images and run on ECS
- Helpful for cost optimizations and focusing less on the infrastructure

### AWS Batch – Simplified Example

AWS Batch

EC2 Instance

Insert

Amazon S3

processed object

Trigger

Spot Instance

Amazon S3


### Batch vs Lambda

- Lambda:
- Time limit
- Limited runtimes
- Limited temporary disk space
- Serverless
- Batch:
- No time limit
- Any runtime as long as it’s packaged as a Docker image
- Rely on EBS / instance store for disk space
- Relies on EC2 (can be managed by AWS)

### Amazon AppFlow

- Fully managed integration service that enables you to securely transfer

data between Software-as-a-Service (SaaS) applications and AWS

- Sources: Salesforce, SAP, Zendesk, Slack, and ServiceNow
- Destinations: AWS services like Amazon S3, Amazon Redshift or non-

AWS such as SnowFlake and Salesforce

- Frequency: on a schedule, in response to events, or on demand
- Data transformation capabilities like filtering and validation
- Encrypted over the public internet or privately over AWS PrivateLink
- Don’t spend time writing the integrations and leverage APIs immediately

### Amazon AppFlow

Sources Destinations

Amazon

AppFlow


### AWS Amplify - web and mobile applications

- A set of tools and services that helps you develop and deploy scalable full stack web and mobile

applications

- Authentication, Storage, API (REST, GraphQL), CI/CD, PubSub, Analytics, AI/ML Predictions,

Monitoring, …

- Connect your source code from GitHub, AWS CodeCommit, Bitbucket, GitLab, or upload directly

connect frontend to backend configure backend

using using

Amplify Frontend Libraries Amplify CLI

Amazon S3 Amazon Cognito AWS API

build using Amplify Console …

AppSync Gateway

& deploy

Amazon Amazon Lex Lambda DynamoDB

… … SageMaker

Frontend Amplify backend

Amplify Amazon

Console CloudFront


### Instance Scheduler on AWS

- AWS solution deployed through CloudFormation

(not a service)

- Automatically start/stop your AWS services to reduce

costs (up to 70%)

- Example: stop company’s EC2 instances outside

business hours

- Supports EC2 instances, EC2 Auto Scaling Groups,

and RDS instances

- Schedules are managed in a DynamoDB table
- Uses resources’ tags and Lambda to stop/start

instances

- Supports cross-account and cross-region resources
- https://aws.amazon.com/solutions/implementations/ins

tance-scheduler-on-aws/


### White Papers & Architectures

Well Architected Framework, Disaster Recovery, etc…


### Section Over view

- Well Architected Framework Whitepaper
- Well Architected Tool
- AWS Trusted Advisor
- Reference architectures resources (for real-world)
- Disaster Recovery on AWS Whitepaper

### Well Architected Framework

General Guiding Principles

- https://aws.amazon.com/architecture/well-architected
- Stop guessing your capacity needs
- Test systems at production scale
- Automate to make architectural experimentation easier
- Allow for evolutionary architectures
- Design based on changing requirements
- Drive architectures using data
- Improve through game days
- Simulate applications for flash sale days

### Well Architected Framework

6 Pillars

- 1) Operational Excellence
- 2) Security
- 3) Reliability
- 4) Performance Efficiency
- 5) Cost Optimization
- 6) Sustainability
- They are not something to balance, or trade-offs, they’re a synergy

### AWS Well-Architected Tool

- Free tool to review your architectures against the 6 pillars Well-Architected

Framework and adopt architectural best practices

- How does it work?
- Select your workload and answer questions
- Review your answers against the 6 pillars
- Obtain advice: get videos and documentations, generate a report, see the results in a dashboard
- Let’s have a look: https://console.aws.amazon.com/wellarchitected

https://aws.amazon.com/blogs/aws/new-aws-well-architected-tool-review-workloads-against-best-practices/


### Trusted Advisor

- No need to install anything – high level

AWS account assessment

- Analyze your AWS accounts and provides

recommendation on 6 categories:

- Cost optimization
- Performance
- Security
- Fault tolerance
- Service limits
- Operational Excellence
- Business & Enterprise Support plan
- Full Set of Checks
- Programmatic Access using AWS Support API

### More Architecture Examples

- We’ve explored the most important architectural patterns:
- Classic: EC2, ELB, RDS, ElastiCache, etc…
- Serverless: S3, Lambda, DynamoDB, CloudFront, API Gateway, etc…
- If you want to see more AWS architectures:
- https://aws.amazon.com/architecture/
- https://aws.amazon.com/solutions/

---

## Exam Review & Tips


### State of learning checkpoint

- Let’s look how far we’ve gone on our learning journey
- https://aws.amazon.com/certification/certified-solutions-architect-

associate/


### Practice makes perfect

- If you’re new to AWS, take a bit of AWS practice thanks to this course

before rushing to the exam

- The exam recommends you to have one or more years of hands-on

experience on AWS

- Practice makes perfect!
- If you feel overwhelmed by the amount of knowledge you just learned,

just go through it one more time


### Proceed by elimination

- Most questions are going to be scenario based
- For all the questions, rule out answers that you know for sure are wrong
- For the remaining answers, understand which one makes the most sense
- There are very few trick questions
- Don’t over-think it
- If a solution seems feasible but highly complicated, it’s probably wrong

### Skim the AWS Whitepapers

- You can read about some AWS White Papers here:
- Architecting for the Cloud: AWS Best Practices
- AWS Well-Architected Framework
- AWS Disaster Recovery (https://aws.amazon.com/disaster-recovery/)
- Overall we’ve explored all the most important concepts in the course
- It’s never bad to have a look at the whitepapers you think are

interesting!


### Read each ser vice’s FAQ

- FAQ = Frequently asked questions
- Example: https://aws.amazon.com/vpc/faqs/
- FAQ cover a lot of the questions asked at the exam
- They help confirm your understanding of a service

### Get into the AWS Community

- Help out and discuss with other people in the course Q&A
- Review questions asked by other people in the Q&A
- Do the practice test in this section
- Read forums online
- Read online blogs
- Attend local meetups and discuss with other AWS engineers
- Watch re-invent videos on Youtube (AWS Conference)

### How will the exam work?

- You’ll have to register online at https://www.aws.training/
- Fee for the exam is 150 USD
- Provide one identity documents (ID, Passport, details are in emails sent to you…)
- No notes are allowed, no pen is allowed, no speaking
- 65 questions will be asked in 130 minutes
- Use the “Flag” feature to mark questions you want to re-visit
- At the end you can optionally review all the questions / answers
- To pass you need a score of a least 720 out of 1000
- You will know within 5 days if you passed / failed the exams (most of the time less)
- You will know the overall score a few days later (email notification)
- You will not know which answers were right / wrong
- If you fail, you can retake the exam again 14 days later

### Your AWS Cer tification journey

Foundational Professional

Knowledge-based certification for Role-based certifications that validate advanced skills

foundational understanding of AWS Cloud. and knowledge required to design secure, optimized,

No prior experience needed. and modernized applications and to automate processes on AWS.

2 years of prior AWS Cloud experience recommended.

Associate Specialty

Role-based certifications that showcase your knowledge Dive deeper and position yourself as a trusted advisor to your

and skills on AWS and build your credibility as an AWS Cloud professional. stakeholders and/or customers in these strategic areas.

Prior cloud and/or strong on-premises IT experience recommended. Refer to the exam guides on the exam pages for recommended experience.


### AWS Cer tification Paths – Architecture

Architecture

Solutions Architect

Design, develop, and manage

cloud infrastructure and assets,

work with DevOps to migrate

applications to the cloud

optional for IT/ recommended for IT/cloud Dive Deep

cloud professionals professionals to leverage AI

Architecture

Application Architect

Design significant aspects of

application architecture including

user interface, middleware, and

infrastructure, and ensure

enterprise-wide scalable, reliable,

and manageable systems optional for IT/ recommended for IT/cloud Dive Deep

cloud professionals professionals to leverage AI

https://d1.awsstatic.com/training-and-

certification/docs/AWS_certification_paths.pdf


### AWS Cer tification Paths – Operations

Operations

Systems Administrator

Install, upgrade, and maintain

computer components and

software, and integrate

automation processes

optional for IT/ Dive Deep

cloud professionals

Operations

Cloud Engineer

Implement and operate an

organization’s networked computing

infrastructure and Implement

security systems to maintain

data safety

optional for IT/ Dive Deep

cloud professionals


### AWS Cer tification Paths – DevOps

DevOps

Test Engineer

Embed testing and quality

best practices for software

development from design to release,

throughout the product life cycle

optional for IT/

cloud professionals

DevOps

Cloud DevOps Engineer

Design, deployment, and operations

of large-scale global hybrid

cloud computing environment,

advocating for end-to-end

automated CI/CD DevOps pipelines

optional for IT/ Optional recommended for IT/cloud Dive Deep

cloud professionals professionals working on

DevOps AI/ML projects

DevSecOps Engineer

Accelerate enterprise cloud adoption

while enabling rapid and stable delivery

of capabilities using CI/CD principles,

methodologies, and technologies

optional for IT/ recommended for IT/cloud

cloud professionals professionals working on

AI/ML projects


### AWS Cer tification Paths – Security

Security

Cloud Security Engineer

Design computer security architecture

and develop detailed cyber security designs.

Develop, execute, and track performance

of security measures to protect information

optional for IT/ recommended for IT/cloud Dive Deep

cloud professionals professionals to secure

AI/ML systems

Security

Cloud Security Architect

Design and implement enterprise cloud

solutions applying governance to identify,

communicate, and minimize business and

technical risks

optional for IT/ recommended for IT/cloud Dive Deep

cloud professionals professionals to secure

AI/ML systems


### AWS Cer tification Paths – Development &

Networking

Development

Software Development Engineer

Develop, construct, and maintain

software across platforms and devices

optional for IT/ recommended for IT/cloud

cloud professionals professionals to leverage AI

Networking

Network Engineer

Design and implement computer

and information networks, such as

local area networks (LAN),

wide area networks (WAN),

optional for IT/ Dive Deep

intranets, extranets, etc.

cloud professionals


### AWS Cer tification Paths – Data Analytics &

AI/ML

Data Analytics

Cloud Data Engineer

Automate collection and processing

of structured/semi-structured data

and monitor data pipeline performance

optional for IT/ recommended for IT/cloud Dive Deep

cloud professionals professionals working on

AI/ML projects

AI/ML

Machine Learning Engineer

Research, build, and design artificial

intelligence (AI) systems to automate

predictive models, and design machine

learning systems, models, and schemes

optional for IT/ optional for AI/ML Dive Deep

cloud professionals professionals


### AWS Cer tification Paths – AI/ML

AI/ML

Prompt Engineer

Design, test, and refine text

prompts to optimize the

performance of AI language models

optional for IT/ Dive Deep

cloud professionals

AI/ML

Machine Learning Ops Engineer

Build and maintain AI and ML platforms

and infrastructure. Design, implement,

and operationally support AI/ML model

activity and deployment infrastructure

optional for IT/ optional for AI/ML

cloud professionals professionals

AI/ML

Data Scientist

Develop and maintain AI/ML models

to solve business problems. Train and

fine tune models and evaluate

their performance

optional for IT/ optional for AI/ML

cloud professionals professionals


---

## Congratulations!


### Congratulations!

- Congrats on finishing the course!
- I hope you will pass the exam without a hitch J
- If you haven’t done so yet, I’d love a review from you!
- If you passed, I’ll be more than happy to know I’ve helped
- Post it in the Q&A to help & motivate other students. Share your tips!
- Post it on LinkedIn and tag me!
- Overall, I hope you learned how to use AWS and that you will be a

tremendously good AWS Solutions Architect

