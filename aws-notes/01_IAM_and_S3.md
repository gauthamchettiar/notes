# IAM
- IAM Terminologies: 
	1. Users
	2. Groups
	3. Policies (JSON Document)
	4. Roles.
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
- **Roles**
	- Roles allow *entities* to temporarily access something
	- The entity can be:
		1. AWS services (EC2, Lambda etc.)
		2. IAM users, groups and Roles in same or different account
		3. Federated Users (AD, LDAP, Web Identity etc.)
	- In order to attach a role to an AWS resource, user must have passRole permission
	- Any resource can only be attached with a single role
- **STS**
	- Similar to Access Key and Secret Access Key, but STS provides a temporary key
	- STS is generlly used for Identity federation
- **Identity Federation**
	- 
	
**Exam Tips:**
- Allows setting up password rotation policy for users.
- Explicit Deny takes precedence over explicit Allows
- More than one policy can be attached to a user at the same time
- Policies cannot be directly attached to any AWS resource (ec2-instance), instead roles has to be created first

# S3
- Stores files like *objects*.
	- Stores them as key-value pairs: 
	Key: file-name
	value: object/file
	version-id: (Important for versioning)
	metadata: (data abut data)
	sub-resources: ACLs, Torrents, etc.
	- Files are stored in virtual folder like storage called *Buckets*
- S3 has a **universal namespace**
- During creation a specific region needs to be choosen, where bucket will be created.
- Example URL:
https://s3-[region-in-which-bucket-is-created].amazonaws.com/[bucket-name]
ie: https://s3-eu-west-1.amazonaws.com/acloudguru
- Files stored are spread accross multiple devices (within same data-center) and multiple facilities (data-center)
- Files can range from *0 Bytes to 5 TB*
- S3 allows *Unlimited Storage*
- OS cannot be installed on S3
- **Data Consistency**
	- *Read After Write* for **NEW PUTS** 
	- *Eventual Consistency* for **OVERWRITE PUTS and DELETES** (Can take some time to propogate)
## S3 Storage Tiers/Classes
1. S3 Standard
	- 99.99% availability
	- 99.99999999999(11x9s) % durability.
	- Minimum Storage Duration : N/A
	- First Byte Latency (Time taken to read the first byte of data) : milliseconds
	- High storage cost but No retrieval Fee
2. S3 : IA (Infrequently Accessed)
	- 99.90% availability
	- 99.99999999999(11x9s) % durability.
	- Minimum Storage Duration : 30 Days
	- First Byte Latency (Time taken to read the first byte of data) : milliseconds
	- Low storage cost but Has retrieval Fee
3. S3 One Zone : IA
	- 99.50% availability
	- 99.99999999999(11x9s) % durability.
	- Minimum Storage Duration : 30 Days
	- First Byte Latency (Time taken to read the first byte of data) : milliseconds
	- Low storage cost but Has retrieval Fee
4. S3 : Intelligent Tiering (Introduced in 2018)
	- Data is moved accross tiers automatically based on usage
	- There is no performance impact or operational overhead
5. S3 Glacier
	- N/A availability
	- 99.99999999999(11x9s) % durability.
	- Minimum Storage Duration : 90 Days
	- First Byte Latency (Time taken to read the first byte of data) : minutes to hours (configurable)
	- Low storage cost but has retrieval Fee
6. S3 Glacier Deep Archive
	- N/A availability
	- 99.99999999999(11x9s) % durability.
	- Minimum Storage Duration : 180 Days
	- First Byte Latency (Time taken to read the first byte of data) : upto 12 hours (configurable)
	- Lowest storage cost but has retrieval Fee
## S3 Features
1. Cross Region Replication (For enhanced durability)
2. S3 Transfer Acceleration (For uploads)
3. S3 Versioning
4. S3 Encryption
5. MFA Delete (For avoiding accidental deletes)
6. S3 Lifecycle Rules

## READ S3 FAQs BEFORE GOING TO EXAM.

**General Exam Tips:**
- When an IAM user tries to access any resource through command line, you require access key id and secret access keys.
-  While using AWS command line utility this credential needs to be stored locally on your machine for the tool to work.
	- This is unsafe for daily use.
- It is recommended that a seperate EC2 isntance be created that has the appropriate role assigned.
- Roles are easier to manage and credentials are stored nowhere on the machine.
- Roles can also be assigned after the machine has been created.
- Roles are UNIVERSAL
