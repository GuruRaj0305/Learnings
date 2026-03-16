## AWS Monitoring, Audit and Performance

CloudWatch, CloudTrail & AWS Config


### Amazon CloudWatch Metrics

- CloudWatch provides metrics for every service in AWS
- Metric is a variable to monitor (CPUUtilization, NetworkIn…)
- Metrics belong to namespaces
- Dimension is an attribute of a metric (instance id, environment, etc.)
- Up to 30 dimensions per metric
- Metrics have timestamps
- Can create CloudWatch dashboards of metrics
- Can create CloudWatch Custom Metrics (for RAM, for example)

### CloudWatch Metric Streams

- Continually stream CloudWatch metrics to a destination of your choice, with near-real-time delivery and low latency
- **Destinations:**
  - Amazon Kinesis Data Firehose (and then its destinations: S3, Redshift, OpenSearch, etc.)
  - 3rd party service providers: Datadog, Dynatrace, New Relic, Splunk, Sumo Logic…
- Option to filter metrics to only stream a subset of them

### CloudWatch Logs

- **Log groups:** Arbitrary name, usually representing an application
- **Log stream:** Instances within application / log files / containers
- Can define log expiration policies (never expire, 1 day to 10 years…)
- CloudWatch Logs can send logs to:
  - Amazon S3 (exports)
  - Kinesis Data Streams
  - Kinesis Data Firehose
  - AWS Lambda
  - OpenSearch
- Logs are encrypted by default
- Can set up KMS-based encryption with your own keys

### CloudWatch Logs – Sources

- SDK, CloudWatch Logs Agent, CloudWatch Unified Agent
- Elastic Beanstalk: collection of logs from application
- ECS: collection from containers
- AWS Lambda: collection from function logs
- VPC Flow Logs: VPC specific logs
- API Gateway
- CloudTrail based on filter
- Route53: Log DNS queries

### CloudWatch Logs Insights

- Search and analyze log data stored in CloudWatch Logs
- Example: find a specific IP inside a log, count occurrences of “ERROR” in your logs
- Provides a purpose-built query language
- Automatically discovers fields from AWS services and JSON log events
- Fetch desired event fields, filter based on conditions, calculate aggregate statistics, sort events, limit number of events
- Can save queries and add them to CloudWatch Dashboards
- Can query multiple Log Groups in different AWS accounts
- It’s a query engine, not a real-time engine
- Reference: https://mng.workshop.aws/operations-2022/detect/cwlogs.html

### CloudWatch Logs – S3 Export

- Log data can take up to 12 hours to become available for export
- The API call is `CreateExportTask`
- Not near-real-time or real-time — use Logs Subscriptions instead

### CloudWatch Logs Subscriptions

- Get real-time log events from CloudWatch Logs for processing and analysis
- Send to Kinesis Data Streams, Kinesis Data Firehose, or Lambda
- **Subscription Filter** – filter which log events are delivered to your destination
- **Destinations (real-time):** Lambda → OpenSearch; Kinesis Data Streams → KDA / EC2 / Lambda
- **Destinations (near-real-time):** Kinesis Data Firehose → OpenSearch

### CloudWatch Logs – Cross-Account Aggregation

- Multi-Account & Multi-Region log aggregation:
  - Account A (Region 1): CloudWatch Logs → Subscription Filter
  - Account B (Region 2): CloudWatch Logs → Subscription Filter
  - Account B (Region 3): CloudWatch Logs → Subscription Filter
  - All converge → Kinesis Data Streams → Kinesis Data Firehose → Amazon S3 (near real-time)
- **Cross-Account Subscription:** The sending account assumes an IAM Role (cross-account) to write to a Subscription Destination (e.g., Kinesis Data Streams) in the receiving account

### CloudWatch Logs for EC2

- By default, no logs from your EC2 machine will go to CloudWatch
- You need to run a **CloudWatch agent** on EC2 to push the log files you want
- Make sure IAM permissions are correct
- The CloudWatch log agent can be set up on-premises too

### CloudWatch Logs Agent & Unified Agent

- For virtual servers (EC2 instances, on-premises servers…)
- **CloudWatch Logs Agent:** Old version; can only send to CloudWatch Logs
- **CloudWatch Unified Agent:**
  - Collects additional system-level metrics such as RAM, processes, etc.
  - Collects logs to send to CloudWatch Logs
  - Centralized configuration using SSM Parameter Store

### CloudWatch Unified Agent – Metrics

Collected directly on your Linux server / EC2 instance:
- CPU (active, guest, idle, system, user, steal)
- Disk metrics (free, used, total); Disk IO (writes, reads, bytes, iops)
- RAM (free, inactive, used, total, cached)
- Netstat (number of TCP and UDP connections, net packets, bytes)
- Processes (total, dead, blocked, idle, running, sleep)
- Swap Space (free, used, used %)
- Reminder: out-of-the-box EC2 metrics – disk, CPU, network (high level only)

### CloudWatch Alarms

- Alarms are used to trigger notifications for any metric
- Various options (sampling, %, max, min, etc.)
- **Alarm States:**
  - `OK`
  - `INSUFFICIENT_DATA`
  - `ALARM`
- **Period:**
  - Length of time in seconds to evaluate the metric
  - High resolution custom metrics: 10 sec, 30 sec, or multiples of 60 sec

### CloudWatch Alarm Targets

- Stop, Terminate, Reboot, or Recover an EC2 Instance
- Trigger Auto Scaling Action
- Send notification to SNS (from which you can do pretty much anything)

### CloudWatch Alarms – Composite Alarms

- CloudWatch Alarms are on a single metric
- Composite Alarms are monitoring the states of multiple other alarms with AND/OR conditions
- Helpful to reduce “alarm noise” by creating complex composite alarms
- Example: Alarm A (CPU) AND Alarm B (IOPS) → Composite Alarm → Amazon SNS notification

### EC2 Instance Recovery

- **Status Checks:**
  - Instance status = check the EC2 VM
  - System status = check the underlying hardware
  - Attached EBS status = check attached EBS volumes
- **CloudWatch Alarm:** monitors `StatusCheckFailed_System`
- **Recovery:** Same Private, Public, Elastic IP, metadata, placement group

### CloudWatch Alarm: Good to Know

- Alarms can be created based on CloudWatch Logs Metrics Filters:
  - CloudWatch Logs → Metric Filter → CloudWatch Alarm → SNS notification
- To test alarms and notifications, set the alarm state to Alarm using CLI:
  ```bash
  aws cloudwatch set-alarm-state --alarm-name "myalarm" --state-value ALARM --state-reason "testing purposes"
  ```

### CloudWatch Network Synthetic Monitor

- Monitor and detect network issues between AWS-hosted apps and on-premises data center
- Identify any network performance degradation (e.g., packet loss, latency, jitter)
- No agents required to be installed
- Tests ICMP or TCP traffic to IPv4/IPv6 on-premises destinations through Direct Connect or S2S VPN connections
- Publishes data to CloudWatch Metrics

### Amazon EventBridge (formerly CloudWatch Events)

- **Schedule:** Cron jobs (scheduled scripts) → e.g., trigger Lambda function every hour
- **Event Pattern:** Event rules to react to a service doing something → e.g., IAM Root User Sign in → SNS Topic with Email Notification
- Trigger Lambda functions, send SQS/SNS messages…

### Amazon EventBridge Rules

**Example Sources:**
- EC2 Instance state change (e.g., Start Instance)
- CodeBuild (e.g., failed build)
- S3 Event (e.g., upload object)
- Trusted Advisor (e.g., new Finding)
- CloudTrail (any API call)
- Schedule or Cron (e.g., every 4 hours)

**Filter events (optional) → JSON event → Example Destinations:**
- Lambda, AWS Batch, ECS Task
- SQS, SNS, Kinesis Data Streams
- Step Functions, CodePipeline, CodeBuild
- SSM, EC2 Actions

### Amazon EventBridge – Event Buses

- **AWS Default Event Bus:** receives events from AWS services
- **Partner Event Bus:** receives events from SaaS partners (e.g., Salesforce, Zendesk)
- **Custom Event Bus:** for your own applications
- Event buses can be accessed by other AWS accounts using Resource-based Policies
- You can archive events (all/filter) sent to an event bus (indefinitely or for a set period)
- Ability to replay archived events

### Amazon EventBridge – Schema Registry

- EventBridge can analyze the events in your bus and infer the schema
- The Schema Registry allows you to generate code for your application, knowing in advance how data is structured in the event bus
- Schema can be versioned

### Amazon EventBridge – Resource-based Policy

- Manage permissions for a specific Event Bus
- Example: allow/deny events from another AWS account or AWS region
- **Use case:** Aggregate all events from your AWS Organization in a single AWS account or AWS region

### CloudWatch Container Insights

- Collect, aggregate, and summarize metrics and logs from containers
- Available for containers on:
  - Amazon ECS (Elastic Container Service)
  - Amazon EKS (Elastic Kubernetes Services)
  - Kubernetes platforms on EC2
  - Fargate (both for ECS and EKS)
- In Amazon EKS and Kubernetes, CloudWatch Insights uses a containerized version of the CloudWatch Agent to discover containers

### CloudWatch Lambda Insights

- Monitoring and troubleshooting solution for serverless applications running on AWS Lambda
- Collects, aggregates, and summarizes system-level metrics: CPU time, memory, disk, and network
- Collects, aggregates, and summarizes diagnostic information: cold starts and Lambda worker shutdowns
- Lambda Insights is provided as a Lambda Layer

### CloudWatch Contributor Insights

- Analyze log data and create time series that display contributor data
- See metrics about the top-N contributors and the total number of unique contributors
- Helps you find top talkers and understand who or what is impacting system performance
- Works for any AWS-generated logs (VPC, DNS, etc.)
- Example: find bad hosts, identify heaviest network users, find URLs generating the most errors
- Can build rules from scratch or use sample rules that AWS has created

### CloudWatch Application Insights

- Provides automated dashboards showing potential problems with monitored applications to isolate ongoing issues
- Your applications run on Amazon EC2 Instances with select technologies (Java, .NET, Microsoft IIS Web Server, databases…)
- Can also use other AWS resources: EBS, RDS, ELB, ASG, Lambda, SQS, DynamoDB, S3, ECS, EKS, SNS, API Gateway…
- Powered by SageMaker
- Enhanced visibility into your application health to reduce time to troubleshoot and repair
- Findings and alerts are sent to Amazon EventBridge and SSM OpsCenter

### CloudWatch Insights and Operational Visibility (Summary)

| Tool | Purpose |
| --- | --- |
| CloudWatch Container Insights | ECS, EKS, Kubernetes on EC2, Fargate; metrics and logs; needs agent for Kubernetes |
| CloudWatch Lambda Insights | Detailed metrics to troubleshoot serverless applications |
| CloudWatch Contributors Insights | Find “Top-N” Contributors through CloudWatch Logs |
| CloudWatch Application Insights | Automatic dashboard to troubleshoot your application and related AWS services |

### AWS CloudTrail

- Provides governance, compliance, and audit for your AWS Account
- CloudTrail is **enabled by default!**
- Get a history of events / API calls made within your AWS Account by: Console, SDK, CLI, AWS Services
- Can put logs from CloudTrail into CloudWatch Logs or S3
- A trail can be applied to All Regions (default) or a single Region
- **If a resource is deleted in AWS, investigate CloudTrail first!**

### CloudTrail Events

- **Management Events:**
  - Operations that are performed on resources in your AWS account
  - Examples: Configuring security (IAM `AttachRolePolicy`), Configuring routing rules (EC2 `CreateSubnet`), Setting up logging (CloudTrail `CreateTrail`)
  - By default, trails are configured to log management events
  - Can separate Read Events (don’t modify resources) from Write Events (may modify resources)
- **Data Events:**
  - By default, data events are not logged (because high volume operations)
  - Amazon S3 object-level activity (e.g., `GetObject`, `DeleteObject`, `PutObject`): can separate Read and Write Events
  - AWS Lambda function execution activity (the `Invoke` API)
- **CloudTrail Insights Events:** see below

### CloudTrail Insights

- Enable CloudTrail Insights to detect unusual activity in your account:
  - Inaccurate resource provisioning
  - Hitting service limits
  - Bursts of AWS IAM actions
  - Gaps in periodic maintenance activity
- CloudTrail Insights analyzes normal management events to create a baseline
- Continuously analyzes write events to detect unusual patterns
- Anomalies appear in the CloudTrail console
- Event is sent to Amazon S3
- An EventBridge event is generated (for automation needs)

### CloudTrail Events Retention

- Events are stored for **90 days** in CloudTrail
- To keep events beyond this period, log them to S3 and use Athena

### Amazon EventBridge – Intercept API Calls

- User calls `DeleteTable` API on DynamoDB
- CloudTrail logs the API call
- EventBridge (triggered on any API call via CloudTrail) → SNS notification (alert)

### Amazon EventBridge + CloudTrail

Examples:
- IAM User assumes role (`AssumeRole`) → CloudTrail logs event → EventBridge → SNS notification
- User edits Security Group Inbound Rules (`AuthorizeSecurityGroupIngress`) → CloudTrail logs event → EventBridge → SNS notification

### AWS Config

- Helps with auditing and recording compliance of your AWS resources
- Helps record configurations and changes over time
- **Questions AWS Config can answer:**
  - Is there unrestricted SSH access to my security groups?
  - Do my buckets have any public access?
  - How has my ALB configuration changed over time?
- You can receive alerts (SNS notifications) for any changes
- AWS Config is a per-region service; can be aggregated across regions and accounts
- Possibility of storing configuration data in S3 (analyzed by Athena)

### Config Rules

- Can use AWS managed config rules (over 75)
- Can make custom config rules (must be defined in AWS Lambda)
  - Example: evaluate if each EBS disk is of type `gp2`
  - Example: evaluate if each EC2 instance is `t2.micro`
- Rules can be evaluated/triggered:
  - For each config change
  - At regular time intervals
- **AWS Config Rules does not prevent actions from happening (no deny)**
- Pricing: no free tier; $0.003 per configuration item recorded per region; $0.001 per config rule evaluation per region

### AWS Config Resource

- View compliance of a resource over time
- View configuration of a resource over time
- View CloudTrail API calls of a resource over time

### Config Rules – Remediations

- Automate remediation of non-compliant resources using SSM Automation Documents
- Use AWS-Managed Automation Documents or create custom Automation Documents
- Tip: create custom Automation Documents that invoke Lambda functions
- You can set Remediation Retries if the resource is still non-compliant after auto-remediation
- Example: IAM Access Key (NON_COMPLIANT) → AWS Config monitors → Auto-Remediation Action triggered (SSM Document: `AWSConfigRemediation-RevokeUnusedIAMUserCredentials`) → deactivate; up to 5 retries

### Config Rules – Notifications

- Use EventBridge to trigger notifications when AWS resources are non-compliant:
  - AWS Config monitors AWS Resources (e.g., security group) → triggers NON_COMPLIANT → EventBridge → Lambda / SNS / SQS
- Ability to send configuration changes and compliance state notifications to SNS (all events):
  - All events (configuration changes, compliance state…) → AWS Config → SNS notification to Admin

### CloudWatch vs CloudTrail vs Config

| Service | Purpose |
| --- | --- |
| CloudWatch | Performance monitoring (metrics, CPU, network, etc.) & dashboards; Events & Alerting; Log Aggregation & Analysis |
| CloudTrail | Record API calls made within your Account by everyone; can define trails for specific resources; Global Service |
| Config | Record configuration changes; evaluate resources against compliance rules; get timeline of changes and compliance |

### For an Elastic Load Balancer (Example)

- **CloudWatch:**
  - Monitoring Incoming connections metric
  - Visualize error codes as % over time
  - Make a dashboard to get an idea of your load balancer performance
- **Config:**
  - Track security group rules for the Load Balancer
  - Track configuration changes for the Load Balancer
  - Ensure an SSL certificate is always assigned to the Load Balancer (compliance)
- **CloudTrail:**
  - Track who made any changes to the Load Balancer with API calls
