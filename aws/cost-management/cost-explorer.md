## Cost Explorer – Monthly Cost by AWS Service


---

## Cost Explorer– Hourly & Resource Level


### Cost Explorer – Savings Plan

Alternative to Reserved Instances


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


### Section Overview

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

