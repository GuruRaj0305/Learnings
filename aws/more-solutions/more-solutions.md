## More Solutions Architecture


### Lambda, SNS & SQS

**Pattern 1 – SQS + Lambda (asynchronous, with DLQ):**

- Lambda polls from SQS
- On failure, Lambda retries; failed messages go to a Dead Letter Queue (DLQ)
- Good for: asynchronous processing with retry

**Pattern 2 – SQS FIFO + Lambda:**

- Messages processed in order
- Lambda polls the FIFO queue; blocking behavior if a message fails

**Pattern 3 – SNS + Lambda:**

- SNS triggers Lambda directly (asynchronous invocation)
- Failed invocations go to a DLQ or SNS retry policy

**Pattern 4 – SNS + SQS FIFO + Lambda:**

- SNS fans out to an SQS FIFO queue; Lambda polls for ordered, reliable processing


### Fan Out Pattern: Deliver to Multiple SQS Queues

**Option 1 – Direct SDK puts (problematic):**

- Application puts messages to SQS #1, #2, #3 separately
- If one put fails, messages are inconsistent

**Option 2 – Fan Out via SNS:**

- Application publishes once to SNS
- SNS fans out to multiple SQS queues via subscriptions
- Reliable, decoupled, each queue receives the same message


### S3 Event Notifications

- Event types: S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication…
- Object name filtering possible (e.g., `*.jpg`)
- Use case: generate thumbnails of images uploaded to S3
- Can create as many “S3 events” as desired
- S3 event notifications typically deliver events in seconds but can sometimes take a minute or longer
- Targets: SNS, SQS, Lambda Function

**S3 Event Notifications with Amazon EventBridge:**

- All S3 events go to EventBridge
- EventBridge rules route events to over 18 AWS services as destinations
- Advanced filtering options with JSON rules (metadata, object size, name…)
- Multiple Destinations – e.g., Step Functions, Kinesis Streams / Firehose
- EventBridge Capabilities: Archive, Replay Events, Reliable delivery


### Amazon EventBridge – Intercept API Calls

- User makes a `DeleteTable` API call to DynamoDB
- CloudTrail logs the API call
- EventBridge rule detects the CloudTrail event
- EventBridge triggers an SNS alert


### API Gateway – AWS Service Integration

**Kinesis Data Streams example:**

- Client sends requests to API Gateway
- API Gateway sends records to Kinesis Data Streams
- Kinesis Data Firehose reads from the stream and stores `.json` files in Amazon S3


### Caching Strategies

Caching can happen at multiple layers to reduce latency and cost:

| Layer | Cache Type | Notes |
| --- | --- | --- |
| CloudFront (edge) | CDN cache | TTL-based, reduces origin requests |
| API Gateway | Response cache | Per-method, TTL configurable |
| Application (EC2 / Lambda) | In-memory | Redis, Memcached |
| Database | Query cache | ElastiCache (Redis/Memcached) in front of RDS/DynamoDB |

Tradeoffs: Network, Computation, Cost, Latency


### Blocking an IP Address

| Scenario | Mechanism |
| --- | --- |
| EC2 directly exposed | NACL Deny rule + Security Group allow rules; optional Firewall Software on EC2 |
| Behind an ALB | NACL on public subnet; ALB terminates connection; EC2 Security Group allows ALB only |
| Behind an NLB | NACL on public subnet (NLB has no Security Group); EC2 Security Group allows NLB |
| ALB + WAF | WAF IP address filtering; NACL optional |
| CloudFront + ALB + WAF | WAF IP address filtering (at edge); Geo Restriction via CloudFront; NACL not helpful (CloudFront IPs) |


### High Performance Computing (HPC)

- The cloud is the perfect place to perform HPC
- You can create a very high number of resources in no time
- You can speed up time to results by adding more resources
- You can pay only for the systems you have used
- Use cases: genomics, computational chemistry, financial risk modeling, weather prediction, machine learning, deep learning, autonomous driving

**Data Management & Transfer:**

- AWS Direct Connect: move GB/s of data to the cloud, over a private secure network
- Snowball & Snowmobile: move PB of data to the cloud
- AWS DataSync: move large amount of data between on-premises and S3, EFS, FSx for Windows

**Compute and Networking:**

- EC2 Instances: CPU optimized, GPU optimized
- Spot Instances / Spot Fleets for cost savings + Auto Scaling
- EC2 Placement Groups: Cluster for good network performance (same rack, same AZ, 10 Gbps network, low latency)
- **EC2 Enhanced Networking (SR-IOV):** Higher bandwidth, higher PPS, lower latency
  - Option 1: Elastic Network Adapter (ENA) – up to 100 Gbps
  - Option 2: Intel 82599 VF – up to 10 Gbps (LEGACY)
- **Elastic Fabric Adapter (EFA):** Improved ENA for HPC, only works for Linux; great for inter-node communications, tightly coupled workloads; leverages MPI standard; bypasses the underlying Linux OS for low-latency, reliable transport

**Storage:**

- Instance-attached storage:
  - EBS: scale up to 256,000 IOPS with io2 Block Express
  - Instance Store: scale to millions of IOPS, linked to EC2 instance, low latency
- Network storage:
  - Amazon S3: large blob, not a file system
  - Amazon EFS: scale IOPS based on total size, or use provisioned IOPS
  - Amazon FSx for Lustre: HPC optimized distributed file system, millions of IOPS, backed by S3

**Automation and Orchestration:**

- **AWS Batch:** Supports multi-node parallel jobs spanning multiple EC2 instances; easily schedule jobs and launch EC2 instances
- **AWS ParallelCluster:** Open-source cluster management tool; configure with text files; automate VPC, Subnet, cluster type and instance types; ability to enable EFA on the cluster


### Creating a Highly Available EC2 Instance

**Basic approach (CloudWatch + Elastic IP):**

- CloudWatch Event (or alarm based on metric) monitors the EC2 instance
- On failure, a Lambda function / script starts a standby EC2 instance and attaches the Elastic IP

**With an Auto Scaling Group:**

- ASG settings: min=1, max=1, desired=1, across ≥2 AZs
- EC2 User Data script attaches the Elastic IP on startup (using EC2 instance role with API call permissions)
- If the instance fails, ASG launches a replacement in another AZ; User Data re-attaches the Elastic IP

**With ASG + EBS:**

- On ASG Terminate lifecycle hook: create an EBS Snapshot (with tags)
- On ASG Launch lifecycle hook: create and attach an EBS volume from the snapshot
- The new EC2 instance has the same data as the previous one


### AWS CloudFormation

- CloudFormation is a declarative way of outlining your AWS Infrastructure, for any resources (most of them are supported)
- Example: in a CloudFormation template, you declare: a security group, two EC2 instances, an S3 bucket, a load balancer – CloudFormation creates those in the right order with the exact configuration

**Benefits:**

- **Infrastructure as code:** No resources are manually created; changes to infrastructure are reviewed through code
- **Cost:** Each resource within the stack is tagged with an identifier; estimate costs using the CloudFormation template; automation (e.g., delete stacks at 5 PM, recreate at 8 AM)
- **Productivity:** Destroy and re-create infrastructure on the fly; automated diagram generation; declarative programming (no need to figure out ordering/orchestration)
- **Leverage existing templates:** Community templates on the web; custom resources for unsupported resources
- **Supports almost all AWS resources**

**CloudFormation + Infrastructure Composer:**

- Visual designer to see all resources and their relations in a CloudFormation stack

**CloudFormation – Service Role:**

- IAM role that allows CloudFormation to create/update/delete stack resources on your behalf
- Gives ability to users to create/update/delete stack resources even if they don’t have direct permissions
- Use case: least privilege principle – user only needs `cloudformation:*`, `iam:PassRole`; CloudFormation’s Service Role does the actual resource operations
- User must have `iam:PassRole` permissions


### Amazon Simple Email Service (Amazon SES)

- Fully managed service to send emails securely, globally and at scale
- Allows inbound/outbound emails
- Reputation dashboard, performance insights, anti-spam feedback
- Provides statistics: email deliveries, bounces, feedback loop results, email open
- Supports DomainKeys Identified Mail (DKIM) and Sender Policy Framework (SPF)
- Flexible IP deployment: shared, dedicated, and customer-owned IPs
- Send emails using AWS Console, APIs, or SMTP
- Use cases: transactional, marketing and bulk email communications


### Amazon Pinpoint

- Scalable 2-way (outbound/inbound) marketing communications service
- Supports email, SMS, push, voice, and in-app messaging
- Ability to segment and personalize messages with the right content to customers
- Possibility to receive replies
- Scales to billions of messages per day
- Use cases: run campaigns by sending marketing, bulk, transactional SMS messages

**Versus Amazon SNS or Amazon SES:**

- In SNS & SES you manage each message’s audience, content, and delivery schedule
- In Amazon Pinpoint, you create message templates, delivery schedules, highly-targeted segments, and full campaigns
- Stream events (e.g., TEXT_SUCCESS, TEXT_DELIVERED) to SNS, Kinesis Data Firehose, CloudWatch Logs


### Systems Manager – SSM Session Manager

- Allows you to start a secure shell on your EC2 and on-premises servers
- No SSH access, bastion hosts, or SSH keys needed; no port 22 needed (better security)
- Supports Linux, macOS, and Windows
- Send session log data to S3 or CloudWatch Logs
- IAM Permissions control who can start sessions (no SSH keypair needed)


### Systems Manager – Run Command

- Execute a document (= script) or just run a command
- Run command across multiple instances (using resource groups)
- No need for SSH
- Command Output can be shown in the AWS Console, sent to S3 bucket or CloudWatch Logs
- Send notifications to SNS about command status (In progress, Success, Failed, …)
- Integrated with IAM & CloudTrail
- Can be invoked using EventBridge


### Systems Manager – Patch Manager

- Automates the process of patching managed instances
- OS updates, application updates, security updates
- Supports EC2 instances and on-premises servers
- Supports Linux, macOS, and Windows
- Patch on-demand or on a schedule using Maintenance Windows
- Scan instances and generate patch compliance report (missing patches)


### Systems Manager – Maintenance Windows

- Defines a schedule for when to perform actions on your instances
- Example: OS patching, updating drivers, installing software, …
- Maintenance Window contains:
  - Schedule
  - Duration
  - Set of registered instances
  - Set of registered tasks


### Systems Manager – Automation

- Simplifies common maintenance and deployment tasks of EC2 instances and other AWS resources
- Examples: restart instances, create an AMI, take EBS snapshot
- **Automation Runbook** – SSM Documents that define actions performed on your EC2 instances or AWS resources (pre-defined or custom)
- Can be triggered by:
  - Manually using AWS Console, AWS CLI or SDK
  - Amazon EventBridge
  - On a schedule using Maintenance Windows
  - By AWS Config for rules remediations


### Cost Explorer

- Visualize, understand, and manage your AWS costs and usage over time
- Create custom reports that analyze cost and usage data
- Analyze your data at a high level: total costs and usage across all accounts
- Monthly, hourly, resource level granularity
- Choose an optimal Savings Plan (to lower prices on your bill)
- Forecast usage up to 18 months based on previous usage


### AWS Well-Architected Framework

**General Guiding Principles** (https://aws.amazon.com/architecture/well-architected):

- Stop guessing your capacity needs
- Test systems at production scale
- Automate to make architectural experimentation easier
- Allow for evolutionary architectures
- Drive architectures using data
- Improve through game days

**6 Pillars:**

1. Operational Excellence
2. Security
3. Reliability
4. Performance Efficiency
5. Cost Optimization
6. Sustainability

They are not trade-offs – they are a synergy.

**AWS Well-Architected Tool:**

- Free tool to review your architectures against the 6 pillars
- Select your workload and answer questions; review against the 6 pillars; obtain advice, generate a report


### Trusted Advisor

- No need to install anything – high level AWS account assessment
- Analyze your AWS accounts and provides recommendations on 6 categories:
  - Cost optimization
  - Performance
  - Security
  - Fault tolerance
  - Service limits
  - Operational Excellence
- Business & Enterprise Support plan: Full Set of Checks + Programmatic Access using AWS Support API


### More Architecture Examples

- Classic: EC2, ELB, RDS, ElastiCache, etc…
- Serverless: S3, Lambda, DynamoDB, CloudFront, API Gateway, etc…
- Reference architectures: https://aws.amazon.com/architecture/
- AWS Solutions: https://aws.amazon.com/solutions/


### Amazon AppFlow

- Fully managed integration service that enables you to securely transfer data between Software-as-a-Service (SaaS) applications and AWS
- Sources: Salesforce, SAP, Zendesk, Slack, and ServiceNow
- Destinations: AWS services like Amazon S3, Amazon Redshift or non-AWS such as SnowFlake and Salesforce
- Frequency: on a schedule, in response to events, or on demand
- Data transformation capabilities like filtering and validation
- Encrypted over the public internet or privately over AWS PrivateLink
- Don’t spend time writing the integrations and leverage APIs immediately


### AWS Amplify – Web and Mobile Applications

- A set of tools and services that helps you develop and deploy scalable full stack web and mobile applications
- Authentication, Storage, API (REST, GraphQL), CI/CD, PubSub, Analytics, AI/ML Predictions, Monitoring, …
- Connect your source code from GitHub, AWS CodeCommit, Bitbucket, GitLab, or upload directly
- Frontend: Amplify Frontend Libraries + Amplify Console (build & deploy)
- Backend: Amplify CLI to configure backend services (S3, Cognito, API Gateway, AppSync, Lambda, DynamoDB, etc.)


### Instance Scheduler on AWS

- AWS solution deployed through CloudFormation (not a service)
- Automatically start/stop your AWS services to reduce costs (up to 70%)
- Example: stop company’s EC2 instances outside business hours
- Supports EC2 instances, EC2 Auto Scaling Groups, and RDS instances
- Schedules are managed in a DynamoDB table
- Uses resources’ tags and Lambda to stop/start instances
- Supports cross-account and cross-region resources
