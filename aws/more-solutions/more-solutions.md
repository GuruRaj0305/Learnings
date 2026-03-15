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


### API Gateway – AWS Service Integration

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


### Other Services

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

### CloudFormation – Service Role

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

