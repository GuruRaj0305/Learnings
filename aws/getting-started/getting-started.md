# Getting Started with AWS


## AWS Cloud History

- **2002**: AWS internally launched
- **2003**: Amazon recognized its infrastructure as a core strength and had the idea to market it
- **2004**: SQS launched publicly
- **2006**: Re-launched publicly with SQS, S3 & EC2
- **2007**: Launched in Europe

## AWS Cloud Number Facts

- In 2023, AWS had $90 billion in annual revenue
- AWS accounts for 31% of the market in Q1 2024 (Microsoft is 2nd with 25%)
- Pioneer and Leader of the AWS Cloud Market for the 13th consecutive year (Gartner Magic Quadrant)
- Over 1,000,000 active users

## AWS Cloud Use Cases

- AWS enables you to build sophisticated, scalable applications
- Applicable to a diverse set of industries
- Use cases include:
  - Enterprise IT, Backup & Storage, Big Data analytics
  - Website hosting, Mobile & Social Apps
  - Gaming

---

## AWS Global Infrastructure

- AWS Regions
- AWS Availability Zones
- AWS Data Centers
- AWS Edge Locations / Points of Presence
- https://infrastructure.aws/

## AWS Regions

- AWS has Regions all around the world
- Names can be us-east-1, eu-west-3…
- A region is a cluster of data centers
- Most AWS services are region-scoped

## How to Choose an AWS Region?

- **Compliance** with data governance and legal requirements: data never leaves a region without your explicit permission
- **Proximity** to customers: reduced latency
- **Available services** within a Region: new services and new features aren't available in every Region
- **Pricing**: pricing varies region to region and is transparent in the service pricing page

## AWS Availability Zones

- Each region has many availability zones (usually 3, min 3, max 6)
- Example: Sydney (ap-southeast-2) has ap-southeast-2a, ap-southeast-2b, ap-southeast-2c
- Each AZ is one or more discrete data centers with redundant power, networking, and connectivity
- They're separate from each other so they're isolated from disasters
- They're connected with high bandwidth, ultra-low latency networking

## AWS Points of Presence (Edge Locations)

- Amazon has 400+ Points of Presence (400+ Edge Locations & 10+ Regional Caches) in 90+ cities across 40+ countries
- Content is delivered to end users with lower latency

## AWS Global vs. Region-Scoped Services

**Global Services:**
- Identity and Access Management (IAM)
- Route 53 (DNS service)
- CloudFront (Content Delivery Network)
- WAF (Web Application Firewall)

**Region-Scoped Services (most AWS services):**
- Amazon EC2 (Infrastructure as a Service)
- Elastic Beanstalk (Platform as a Service)
- Lambda (Function as a Service)
- Rekognition (Software as a Service)
- Region table: https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services

---

## IAM: Users & Groups

- IAM = Identity and Access Management, Global service
- Root account created by default, shouldn't be used or shared
- Users are people within your organization, and can be grouped
- Groups only contain users, not other groups (no nested groups)
- Users don't have to belong to a group, and a user can belong to multiple groups

## IAM: Permissions

- Users or Groups can be assigned JSON documents called **policies**
- These policies define the permissions of the users
- In AWS you apply the **least privilege principle**: don't give more permissions than a user needs

## IAM Policies Inheritance

- Policies attached at the group level are inherited by all members of that group
- Users can also have inline policies attached directly to them
- A user in multiple groups inherits all policies from each group

## IAM Policies Structure

Consists of:
- **Version**: policy language version, always include `"2012-10-17"`
- **Id** (optional): an identifier for the policy
- **Statement**: one or more individual statements
  - **Sid** (optional): an identifier for the statement
  - **Effect**: whether the statement allows or denies access (`Allow` / `Deny`)
  - **Principal**: account/user/role to which this policy applies
  - **Action**: list of actions this policy allows or denies
  - **Resource**: list of resources to which the actions apply
  - **Condition** (optional): conditions for when this policy is in effect

## IAM – Password Policy

- Strong passwords = higher security for your account
- In AWS, you can set up a password policy:
  - Set a minimum password length
  - Require specific character types (uppercase, lowercase, numbers, non-alphanumeric)
  - Allow all IAM users to change their own passwords
  - Require users to change their password after some time (password expiration)
  - Prevent password re-use

## Multi-Factor Authentication (MFA)

- MFA = password you know + security device you own
- Main benefit: if a password is stolen or hacked, the account is not compromised

**MFA device options in AWS:**
- **Virtual MFA device**: Google Authenticator (phone only), Authy (supports multiple tokens on a single device)
- **Universal 2nd Factor (U2F) Security Key**: YubiKey by Yubico (3rd party, physical device, supports multiple root and IAM users using a single key)
- **Hardware Key Fob MFA Device**: Provided by Gemalto (3rd party)
- **Hardware Key Fob MFA Device for AWS GovCloud (US)**: Provided by SurePassID (3rd party)

## How Can Users Access AWS?

- **AWS Management Console**: protected by password + MFA
- **AWS Command Line Interface (CLI)**: protected by access keys
- **AWS Software Developer Kit (SDK)**: for code, protected by access keys
- Access Keys are generated through the AWS Console
- Users manage their own access keys
- Access Keys are secret, just like a password — never share them
- Access Key ID ≈ username, Secret Access Key ≈ password

## What's the AWS CLI?

- A tool that enables you to interact with AWS services using commands in your command-line shell
- Direct access to the public APIs of AWS services
- You can develop scripts to manage your resources
- It's open-source: https://github.com/aws/aws-cli
- Alternative to using the AWS Management Console

## What's the AWS SDK?

- AWS Software Development Kit (AWS SDK)
- Language-specific APIs (set of libraries)
- Enables you to access and manage AWS services programmatically from within your application
- Supports: JavaScript, Python, PHP, .NET, Ruby, Java, Go, Node.js, C++, Mobile SDKs (Android, iOS), IoT Device SDKs (Embedded C, Arduino)
- Example: AWS CLI is built on AWS SDK for Python

## IAM Roles for Services

- Some AWS services need to perform actions on your behalf
- To do so, we assign permissions to AWS services with **IAM Roles**
- Common roles:
  - EC2 Instance Roles
  - Lambda Function Roles
  - Roles for CloudFormation

## IAM Security Tools

- **IAM Credentials Report** (account-level): a report that lists all account's users and the status of their various credentials
- **IAM Access Advisor** (user-level): shows the service permissions granted to a user and when those services were last accessed — useful for revising policies

## IAM Guidelines & Best Practices

- Don't use the root account except for AWS account setup
- One physical user = One AWS user
- Assign users to groups and assign permissions to groups
- Create a strong password policy
- Use and enforce the use of MFA
- Create and use Roles for giving permissions to AWS services
- Use Access Keys for Programmatic Access (CLI/SDK)
- Audit permissions of your account using IAM Credentials Report & IAM Access Advisor
- Never share IAM users & Access Keys

## IAM Section – Summary

| Component   | Description |
|-------------|-------------|
| Users       | Mapped to a physical user; has a password for AWS Console |
| Groups      | Contains users only |
| Policies    | JSON document that outlines permissions for users or groups |
| Roles       | For EC2 instances or AWS services |
| Security    | MFA + Password Policy |
| AWS CLI     | Manage your AWS services using the command-line |
| AWS SDK     | Manage your AWS services using a programming language |
| Access Keys | Access AWS using the CLI or SDK |
| Audit       | IAM Credential Reports & IAM Access Advisor |
