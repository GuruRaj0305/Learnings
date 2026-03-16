## Advanced Identity in AWS


### AWS Organizations

- Global service
- Allows to manage multiple AWS accounts
- The main account is the management account
- Other accounts are member accounts
- Member accounts can only be part of one organization
- Consolidated Billing across all accounts – single payment method
- Pricing benefits from aggregated usage (volume discount for EC2, S3…)
- Shared reserved instances and Savings Plans discounts across accounts
- API is available to automate AWS account creation

**Organizational Structure Example:**

```
Root OU
  Management Account
  OU (Dev)
    Member Accounts
  OU (Prod)
    OU (HR)
    OU (Finance)
    Member Accounts
```

**Advantages:**

- Multi Account vs One Account Multi VPC
- Use tagging standards for billing purposes
- Enable CloudTrail on all accounts, send logs to central S3 account
- Send CloudWatch Logs to central logging account
- Establish Cross Account Roles for Admin purposes

**Security: Service Control Policies (SCP)**

- IAM policies applied to OU or Accounts to restrict Users and Roles
- They do not apply to the management account (full admin power)
- Must have an explicit allow from the root through each OU in the direct path to the target account (does not allow anything by default – like IAM)

**Organizational Units (OU) – Common Patterns:**

- **Business Unit:** Sales OU, Retail OU, Finance OU – group by business function
- **Environmental Lifecycle:** Prod OU, Dev OU, Test OU – group by environment
- **Project-Based:** Separate OUs per project with dedicated member accounts


### SCP Hierarchy

SCPs work on a hierarchy – a Deny at a higher level overrides an Allow below.

**Example:**

- **Management Account:** Can do anything (no SCPs apply)
- **Sandbox OU** has `FullAWSAccess + Deny S3`:
  - **Account A:** Can do anything EXCEPT S3 (explicit Deny from Sandbox OU)
- **Workloads OU** has `FullAWSAccess + Deny EC2`:
  - **Test OU** inherits Deny EC2; **Account D** can access EC2 because Test OU has `Allow EC2` (overrides)
  - **Prod OU** has `FullAWSAccess`; Accounts E & F can do anything; Account B & C EXCEPT S3

**SCP Rules:**

- Default is `FullAWSAccess` (allow all) at root
- Add Deny policies at any OU level to restrict
- An explicit Deny at a parent OU propagates down to all child OUs and accounts

More examples: https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_example-scps.html


### AWS Organizations – Tag Policies

- Helps you standardize tags across resources in an AWS Organization
- Ensure consistent tags, audit tagged resources, maintain proper resources categorization
- You define tag keys and their allowed values
- Helps with AWS Cost Allocation Tags and Attribute-based Access Control
- Prevent any non-compliant tagging operations on specified services and resources (has no effect on resources without tags)
- Generate a report that lists all tagged/non-compliant resources
- Use EventBridge to monitor non-compliant tags


### IAM Conditions

| Condition Key | Purpose |
| --- | --- |
| `aws:SourceIp` | Restrict the client IP from which the API calls are being made |
| `aws:RequestedRegion` | Restrict the region the API calls are made to |
| `ec2:ResourceTag` | Restrict based on tags |
| `aws:MultiFactorAuthPresent` | Force MFA |


### IAM for S3

- `s3:ListBucket` permission applies to `arn:aws:s3:::test` → **bucket level permission**
- `s3:GetObject`, `s3:PutObject`, `s3:DeleteObject` applies to `arn:aws:s3:::test/*` → **object level permission**


### Resource Policies & aws:PrincipalOrgID

- `aws:PrincipalOrgID` can be used in any resource policy to restrict access to accounts that are members of an AWS Organization
- Example: an S3 bucket policy using `aws:PrincipalOrgID` will deny access to any principal outside the organization


### IAM Roles vs. Resource-Based Policies

**Cross-account access options:**

1. Attach a resource-based policy to a resource (e.g., S3 bucket policy)
2. Use a role as a proxy (user assumes a role in the target account)

**Key difference:**

- When you **assume a role** (user, application or service), you give up your original permissions and take the permissions assigned to the role
- When using a **resource-based policy**, the principal doesn’t have to give up their permissions
- Example: User in Account A needs to scan a DynamoDB table in Account A and dump it in an S3 bucket in Account B – use a resource-based policy on the S3 bucket

**Supported by:** Amazon S3 buckets, SNS topics, SQS queues, and more


### Amazon EventBridge – Security

- When a rule runs, it needs permissions on the target
- **Resource-based policy:** Lambda, SNS, SQS, S3 buckets, API Gateway – allow EventBridge to invoke the target
- **IAM role:** EC2 Auto Scaling, Systems Manager Run Command, ECS task – EventBridge assumes the role to invoke the target


### IAM Permission Boundaries

- IAM Permission Boundaries are supported for users and roles (not groups)
- Advanced feature to use a managed policy to set the **maximum permissions** an IAM entity can get
- Effective permissions = intersection of IAM Permission Boundary ∩ IAM Policy

**Use cases:**

- Delegate responsibilities to non-administrators within their permission boundaries (e.g., create new IAM users)
- Allow developers to self-assign policies and manage their own permissions, while making sure they can’t “escalate” their privileges (= make themselves admin)
- Useful to restrict one specific user (instead of a whole account using Organizations & SCP)

Reference: https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html


### IAM Policy Evaluation Logic

Reference: https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html


### Example IAM Policy

- Can you perform `sqs:CreateQueue`?
- Can you perform `sqs:DeleteQueue`?
- Can you perform `ec2:DescribeInstances`?


### AWS IAM Identity Center

(successor to AWS Single Sign-On)

- One login (single sign-on) for all your:
  - AWS accounts in AWS Organizations
  - Business cloud applications (e.g., Salesforce, Box, Microsoft 365, …)
  - SAML2.0-enabled applications
  - EC2 Windows Instances
- Identity providers:
  - Built-in identity store in IAM Identity Center
  - 3rd party: Active Directory (AD), OneLogin, Okta…

**Login Flow:**

1. User opens the IAM Identity Center browser interface
2. Authenticates against the identity store (built-in or Active Directory)
3. Receives a permission set – an IAM role – for the target account or application
4. Single sign-on access to AWS accounts, business apps, and SAML2.0-enabled apps

**Architecture:**

- IAM Identity Center connects to an AWS Organization
- Users and groups are stored in the built-in Identity Store (or synced from Active Directory on-premises/cloud)
- Permission Sets define the IAM policies for each user/group per account


### IAM Identity Center – Fine-Grained Permissions and Assignments

**Multi-Account Permissions:**

- Manage access across AWS accounts in your AWS Organization
- Permission Sets – a collection of one or more IAM Policies assigned to users and groups to define AWS access

**Application Assignments:**

- SSO access to many SAML 2.0 business applications (Salesforce, Box, Microsoft 365, …)
- Provide required URLs, certificates, and metadata

**Attribute-Based Access Control (ABAC):**

- Fine-grained permissions based on users’ attributes stored in IAM Identity Center Identity Store
- Example attributes: cost center, title, locale, …
- Use case: Define permissions once, then modify AWS access by changing the attributes

**Example:** Group “Developers” (Bob, Alice) in the IAM Identity Center → assigned a `ReadOnlyAccess` Permission Set to Dev Accounts, `FullAccess` Permission Set to Prod Accounts.


### What is Microsoft Active Directory (AD)?

- Found on any Windows Server with AD Domain Services
- Database of objects: User Accounts, Computers, Printers, File Shares, Security Groups
- Centralized security management, create account, assign permissions
- Objects are organized in trees; a group of trees is a forest


### AWS Directory Services

- **AWS Managed Microsoft AD**
  - Create your own AD in AWS, manage users locally, supports MFA
  - Establish “trust” connections with your on-premises AD
- **AD Connector**
  - Directory Gateway (proxy) to redirect to on-premises AD, supports MFA
  - Users are managed on the on-premises AD
- **Simple AD**
  - AD-compatible managed directory on AWS
  - Cannot be joined with on-premises AD


### IAM Identity Center – Active Directory Setup

**Option 1: Connect to an AWS Managed Microsoft AD (Directory Service)**

- Integration is out of the box
- IAM Identity Center connects directly to AWS Managed Microsoft AD

**Option 2: Connect to a Self-Managed Directory**

- Create a Two-way Trust Relationship using AWS Managed Microsoft AD
- Or create an AD Connector (proxy) that forwards authentication to the self-managed AD


### AWS Control Tower

- Easy way to set up and govern a secure and compliant multi-account AWS environment based on best practices
- AWS Control Tower uses AWS Organizations to create accounts
- Benefits:
  - Automate the set up of your environment in a few clicks
  - Automate ongoing policy management using guardrails
  - Detect policy violations and remediate them
  - Monitor compliance through an interactive dashboard


### AWS Control Tower – Guardrails

- Provides ongoing governance for your Control Tower environment (AWS Accounts)
- **Preventive Guardrail** – using SCPs (e.g., Restrict Regions across all your accounts)
- **Detective Guardrail** – using AWS Config (e.g., identify untagged resources)

**Flow:** Guardrail (Detective) detects a non-compliant resource → AWS Config triggers notification → Lambda function remediates (e.g., adds tags) → Admin is notified via SNS
