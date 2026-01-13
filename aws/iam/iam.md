# IAM ( Identity and Access Management )

+ Global service
+ Users and Groups
+ Root account created by default, should not be used or shared
+ Users are people with in your org, and can be grouped
+ Groups only contains users, not other groups
+ Users dont have to belong to a group ( not best practice but possible ), user can belongs to multiple group


## IAM Permissions

+ Users or group can be assignes json document called policies.
+ This policies defines the permission of the users.
+ In AWS you apply the **least privilege principle** : dont give more permission than user needs.



## IAM Policies Structure
Consists of:
  - version: policy language version, always include "2012-10-17"
  - id (optional): an identifier for the policy 
  - Statement : one or more individual statements
    - sid (optional) : an identifier for the statement.
    - effect : whether the statement allows or denies access.
    - principle : account/user/role which policy applied to.
    - action : list of actions this policy allows or denies.
    - resource : list of resource this policy applied to.
    - condition (optional) : conditions for which this policy is in effect.
  

## IAM Passowrd policy
In aws we can set up a passwork policy:
+ set min password length.
+ required specific char type.
+ allow iam users to change their own password
+ require users to change password, after password expiry.
+ prevent users to re-use

## IAM MFA (Multi Factor Authentication )
+ MFA = password you know + secure password you own
+ **MFA devices options in aws** : 
  + Virtual MFA Device: 
    + Google authenticator (phone only)
    + Authy (phone only) [ supports multiple tokens in single device (multiple users) ]
  + Universal 2nd Factor ( U2F ) Security key.
    + Yubikey by yubico ( 3rd party ) ( phsical device )
  + Hardware Key Fob MFA Device, Proviced by Gemolto ( 3rd party ) ( phsical device )
  + Hardware Key Fob MFA Device for AWS GovCloud(US)  ( phsical device )



## IAM Roles for Services

+ Some AWS service will need to perform actions on your behalf
+ To do so, we will assign permissions to AWS services with IAM Roles.
+ Common roles:
  + EC2 Instance Roles
  + Lambda functions Roles
  + Roles for CloudFormation


## IAM Security Tools

+ IAM Credentials Report ( account-level )
  + a report that lists all account's users and the status of their various credentials
+ IAM Access Advisor ( User-level )
  + Access advisor shows the service permissions granted to a user and when those services were last accessed.
  + You can use this information to revise your policies.


## IAM Guidelines & Best Practices
+ Dont use the root account except for AWS acount setup
+ One physical user = One AWS user
+ Assign users to groups and assign permissions to groups
+ Create a strong password policy
+ Use and enforce the use of MFA
+ Create and use Roles for giving permissions to AWS services
+ Use Access Keys for Programmatic Access (CLI/SDK)
+ Audit permissions of your account using IAM Credentials Report & IAM Access Advisor
+ Never share IAM users & Access Keys