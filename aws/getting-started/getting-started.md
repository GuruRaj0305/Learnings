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

### IAM Roles for Services

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

