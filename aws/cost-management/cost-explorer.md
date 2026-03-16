## Cost Management


### AWS Cost Explorer

- Visualize, understand, and manage your AWS costs and usage over time
- Create custom reports that analyze cost and usage data
- Analyze your data at a high level: total costs and usage across all accounts
- Monthly, hourly, resource-level granularity
- Choose an optimal Savings Plan (to lower prices on your bill)
- Forecast usage up to 18 months based on previous usage
- View types: Monthly Cost by AWS Service, Hourly & Resource Level, Savings Plan recommendations, Forecast Usage


### AWS Cost Anomaly Detection

- Continuously monitor your cost and usage using ML to detect unusual spends
- Learns your unique, historic spend patterns to detect one-time cost spikes and/or continuous cost increases (no need to define thresholds)
- Monitor AWS services, member accounts, cost allocation tags, or cost categories
- Sends you the anomaly detection report with root-cause analysis
- Get notified with individual alerts or daily/weekly summary (using SNS)

**Workflow:**
1. Create Cost Monitor – identify unusual spend at the granularity level you specify
2. Get Alerted – receive alerts when unusual spend is detected
3. Analyze Root Cause – analyze the root cause behind the anomaly and the impact on your costs


### AWS Outposts

- Hybrid Cloud: businesses that keep an on-premises infrastructure alongside a cloud infrastructure
- Two ways of dealing with IT systems: one for the AWS cloud (console, CLI, APIs) and one for their on-premises infrastructure
- AWS Outposts are “server racks” that offer the same AWS infrastructure, services, APIs & tools to build your own applications on-premises just as in the cloud
- AWS will set up and manage “Outposts Racks” within your on-premises infrastructure
- You are responsible for the Outposts Rack physical security

**Benefits:**

- Low-latency access to on-premises systems
- Local data processing
- Data residency
- Easier migration from on-premises to the cloud
- Fully managed service

**Services that work on Outposts:** Amazon EC2, Amazon EBS, Amazon S3, Amazon EKS, Amazon ECS, Amazon RDS, Amazon EMR


### AWS Batch

- Fully managed batch processing at any scale
- Efficiently run 100,000s of computing batch jobs on AWS
- A “batch” job is a job with a start and an end (opposed to continuous)
- Batch will dynamically launch EC2 instances or Spot Instances
- AWS Batch provisions the right amount of compute / memory
- You submit or schedule batch jobs and AWS Batch does the rest!
- Batch jobs are defined as Docker images and run on ECS
- Helpful for cost optimizations and focusing less on the infrastructure

**Batch vs Lambda:**

| Feature | Lambda | Batch |
| --- | --- | --- |
| Time limit | Yes (15 min max) | No time limit |
| Runtime | Limited runtimes | Any runtime (Docker image) |
| Disk space | Limited temporary disk | EBS / instance store |
| Compute | Serverless | EC2 (can be managed by AWS) |


### White Papers & Architectures

- **Well-Architected Framework Whitepaper:** https://aws.amazon.com/architecture/well-architected
- **Well-Architected Tool:** Free tool to review architectures against the 6 pillars
- **AWS Trusted Advisor:** Account-level assessment on cost, performance, security, fault tolerance, service limits, operational excellence
- **Reference architectures:** https://aws.amazon.com/architecture/
- **AWS Solutions:** https://aws.amazon.com/solutions/
- **Disaster Recovery on AWS Whitepaper:** https://aws.amazon.com/disaster-recovery/


### Section Overview

- Well Architected Framework Whitepaper
- Well Architected Tool
- AWS Trusted Advisor
- Reference architectures resources (for real-world)
- Disaster Recovery on AWS Whitepaper


### Well Architected Framework – General Guiding Principles

- Stop guessing your capacity needs
- Test systems at production scale
- Automate to make architectural experimentation easier
- Allow for evolutionary architectures – Design based on changing requirements
- Drive architectures using data
- Improve through game days – Simulate applications for flash sale days


### Well Architected Framework – 6 Pillars

1. Operational Excellence
2. Security
3. Reliability
4. Performance Efficiency
5. Cost Optimization
6. Sustainability

They are not trade-offs; they are a synergy.


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
