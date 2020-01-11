# Contents

## IAM

- [IAM Terminologies](01_IAM.md#iam-terminologies)
- [IAM Basics](01_IAM.md#iam-basics)
- [IAM Roles](01_IAM.md#iam-roles)
- [STS](01_IAM.md#sts)
- [IAM Exam Tips](01_IAM.md#iam-exam-tips)
- [IAM Best Practice - Using Roles instead of Access Keys](01_IAM.md#iam-best-practice---using-roles-instead-of-access-keys)

## S3

- [S3 Introduction](02_S3.md#what-is-s3)
- [S3 Storge Classes/Tiers](02_S3.md#s3-storage-tiersclasses)
	1. S3 Standard
	2. S3 : IA (Infrequently Accessed)
	3. S3 One Zone : IA
	4. S3 : Intelligent Tiering (Introduced in 2018)
	5. S3 Glacier
	6. S3 Glacier Deep Archive
- [List of S3 features](02_S3.md#s3-features)
	1. [Cross Region Replication (For enhanced durability)](02_S3.md#s3-cross-region-replication-crr)
	2. [S3 Transfer Acceleration (For uploads)](02_S3.md#s3-transfer-acceleration)
	3. [S3 Versioning](02_S3.md#s3-versioning)
	4. [S3 Encryption](02_S3.md#s3-encryption)
		- S3 Managed Keys
		- AWS KMS
		- Server Side Encryption
	5. MFA Delete (For avoiding accidental deletes)
	6. [S3 Lifecycle Rules](02_S3.md#s3-lifecycle-rules)
	7. [S3 Access Control](02_S3.md#s3-access-control)
		- Bucket Policies
		- Access Control Lists
	8. [S3 Logging](02_S3.md#s3-logging)
	9. [S3 Multipart Uploads](02_S3.md#s3-multipart-uploads)

- [Content Delivery Network (CDN)](02_S3.md#cdn---content-delivery-network)
- [Edge Location (Read & Write)](02_S3.md#edge-location-read--write)
- [Snowball](02_S3.md#snowball)
- [Storage Gateway](02_S3.md#storage-gateway)
	1. File Gateway
	2. Volume Gateway
	3. Tape Gateway
- [FAQ](02_S3.md#from-faqs)
	1. How should one choose which region to store his/her data in?
	2. S3 Pricing model?
	3. How am I charged if my Amazon S3 buckets are accessed from another AWS account?
	4. Setting up trash, recycle bin, or rollback window on my Amazon S3 objects to recover from deletes and overwrites?
	5. What happens when one deletes an object from a storage before it's minimum storage time?

## EC2

- [EC2 Introduction](03_EC2.md#what-is-ec2)
- [Types of EC2 Pricing Models]
(03_EC2.md#types-of-ec2-pricing-models)
	1. On Demand
	2. Reserved
	3. Spot
	4. Dedicated Hosts
- [When to use what?](03_EC2.md#when-to-use-what)
- [EC2 Instance Types](03_EC2.md#ec2-instance-types)</br>
	F1, I3, G3, H1, T3, D2, R5, M5, C5, P3, X1, Z1D, A1, U-6tb1
- [EC2 Instance Creation](03_EC2.md#ec2-instance-creation)
- [EC2 Termination Protection](03_EC2.md#ec2-termination-protection)
- [EC2 Volume Encryption](03_EC2.md#ec2-volume-encryption)
- [EC2 Bootstrapping](03_EC2.md#ec2-bootstrapping)
- [EC2 Exam Tips](03_EC2.md#ec2-exam-tips)

## Security Groups

- [Introduction](03_EC2.md#security-groups)
- [Exam Tips](03_EC2.md#security-groups-exam-tips)

## AWS CloudWatch

- [Introduction](03_EC2.md#aws-cloudwatch)

## AWS Cloud Trial

- [Introduction](03_EC2.md#aws-cloud-trial)

## EBS

- Types of EBS Volume
	1. General Purpose (SSD)
	2. Provisioned IOPS (SSD)
	3. Throughput Optimised Hard Disk Drive (Magnetic)
	4. Cold Hard Disk Drive
	5. Magnetic
- Snapshots
- AMI
- AMI/EC2 Instance Volume Backing
	1. EBS Volume
	2. Instance Store Volume
- EFS
- Placement groups
	1. Cluster
	2. Partition
	3. Spread

# RDS

