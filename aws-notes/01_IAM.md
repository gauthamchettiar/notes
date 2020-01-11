# IAM - Identity and Access Management

## IAM Terminologies

  1. Users
  2. Groups
  3. Policies (JSON Document)
  4. Roles.

## IAM Basics

- IAM is **UNIVERSAL**.
- **root account** created when account is first setup.
- This account has complete admin access
- It is recommended to setup MFA on this account.
- It is also recommended to reduce or not use this account to manage AWS.
- **new users** can be created through IAM.
- They have no permission when first created.
- *(Optionally)* Users can be assigned Access Key Id & Secret Access Keys.
  - These Keys can be used to login to AWS via APIs and Command Lines.
  - These Keys can only be viewed once (If lost a new one has to be generated).

## IAM Roles

- Roles allow *entities* to temporarily access something
- The entity can be:
  1. AWS services (EC2, Lambda etc.)
  2. IAM users, groups and Roles in same or different account
  3. Federated Users (AD, LDAP, Web Identity etc.)
- In order to attach a role to an AWS resource, user must have passRole permission
  - Any resource can only be attached with a single role
  - Roles can also be assigned after the machine has been created.
  - Roles are UNIVERSAL

## STS

- Similar to Access Key and Secret Access Key, but STS provides a temporary key
- STS is generlly used for Identity federation

## IAM Exam Tips

- Allows setting up password rotation policy for users.
- Explicit Deny takes precedence over explicit Allows
- More than one policy can be attached to a user at the same time
- Policies cannot be directly attached to any AWS resource (ec2-instance), instead roles has to be created first

### IAM Best Practice - Using Roles instead of Access Keys

- When an IAM user tries to access any resource through command line, you require access key id and secret access keys.
- While using AWS command line utility this credential needs to be stored locally on your machine for the tool to work.
  - This is unsafe for daily use.
- It is recommended that a seperate EC2 isntance be created that has the appropriate role assigned.
- Roles are easier to manage and credentials are stored nowhere on the machine.
