# IAM (Identity and Access Management)

- Global service
- Root account created by default, should not be used or shared
- Users are people within your org, and can be grouped
- Groups only contain users, not other groups
- Users don't have to belong to a group (not best practice, but possible); a user can belong to multiple groups

## IAM Permissions

- Users or groups can be assigned JSON documents called policies
- These policies define the permissions of the users
- In AWS you apply the **least privilege principle**: don't give more permissions than a user needs

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

## IAM Password Policy

In AWS we can set up a password policy:
- Set minimum password length
- Require specific character types
- Allow IAM users to change their own password
- Require users to change their password after it expires
- Prevent users from re-using previous passwords

## IAM MFA (Multi-Factor Authentication)

- MFA = password you know + security device you own
- **MFA device options in AWS**:
  - **Virtual MFA Device**:
    - Google Authenticator (phone only)
    - Authy (supports multiple tokens on a single device)
  - **Universal 2nd Factor (U2F) Security Key**:
    - YubiKey by Yubico (3rd party, physical device)
  - **Hardware Key Fob MFA Device**: Provided by Gemalto (3rd party, physical device)
  - **Hardware Key Fob MFA Device for AWS GovCloud (US)** (physical device)



## IAM Roles for Services

- Some AWS services need to perform actions on your behalf
- To do so, we assign permissions to AWS services with IAM Roles
- Common roles:
  - EC2 Instance Roles
  - Lambda Function Roles
  - Roles for CloudFormation


## IAM Security Tools

- **IAM Credentials Report** (account-level): a report that lists all account's users and the status of their various credentials
- **IAM Access Advisor** (user-level): shows the service permissions granted to a user and when those services were last accessed; use this information to revise your policies

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