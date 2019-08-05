# EBS
- Persistent block storage (not object storage) volumes that can be used with EC2
- Each EBS volume is replicated within its AZ for protection against component failure
## Types of EBS Storage
1. General Purpose (SSD):
	- Used for most workloads
	- API NAME: gp2
2. Provisioned IOPS (SSD):
	- Useful for Databases
	- API NAME: io1
3. Throughput Optimised Hard Disk Drive (Magnetic):
	- Useful for Big Data and Data Ware houses
	- API NAME: st1
4. Cold Hard Disk Drive:
	- Suitable for File Servers
	- API NAME: sc1
5. Magnetic:
	- Useful for workloads with infrequently accessed data
	- API NAME: Standard

# Snapshots
- Snapshots are point in time backups of any volume.
- Snapshots exist on S3.
- Snapshots are incremental, only changed blocks since last snapshot are saved.
	- Thus first Snapshots take time to create and subsequent snapshots are faster
- It is recommended to stop the instance before creation of snapshot, so that the state is consistent.
	- However, it is possible to create snapshots of a running instance.

# AMI
- An Amazon Machine Image (AMI) provides the information required to launch an EC2 instance. 
- It contains information such as:
	- One or more EBS Snapshots of the root volume of the instance.
	- Launch permissions.
	- A Block device mappings (volumes to attach to instance on launch)
- Creation of AMI from a running instance: 
	1. Create a snapshot of the entire EC2 instance.
	2. Register an AMI from the created snapshot.
	OR
	1. Register an AMI directly from the EBS volume attached to a running AMI.
- Operations possible with created AMI:
	1. Create another EC2 instance.
	2. Copy to create a new AMI.
	3. Deregister AMI.
	4. Share with Others (One can also buy or sell AMIs on Amazon Marketplace)
## AMI/EC2 Instance Volume Backing
1. EBS Volume: 
	- Instance type of root volume is Amazon's EBS Volume, which is created from EBS Snapshot.
	- EC2 instances backed by EBS can be stopped, any data stored will be restored when restarted.
2. Instance Store Volumes: 
	- Instance type of root volume is an Instance Store, created from templates.
	- Instance stores are *ephemeral*, there is no record of instance stores anywhere and once the machine is terminated adjoining instance store volume is also deleted.
	- EC2 instances backed by Instance stores cannot be stopped, they can only be terminated.
	- *You can reboot EC2 instances backed by both.*

**EC2, EBS, Snapshot & AMI Exam Tips:**
1. EBS volume sizes can be changed on the fly, changes include changing the storage size and storage type.
2. Volumes created for any EC2 instance will be on the same Region as the corresponding EC2 instance.
3. MOVING AN EC2 INSTANCE FROM ONE AZ TO ANOTHER:
	- EC2 Instance => Snapshot => AMI
		- Create the new EC2 instance in the new AZ using the created AMI
4. MOVING AN EC2 INSTANCE FROM ONE REGION TO ANOTHER:
	- EC2 Instance => Snapshot => AMI
		- Copy the AMI to the desired region
		- Create the new EC2 instance from the copied AMI
5. **Encryption:**
	A. Snapshots of ENCRYPTED volumes are ENCRYPTED.
	B. Volumes restored from ENCRYPTED snapshots are ENCRYPTED by default.
	C. Only UNENCRYPTED snapshots can be shared.
		- ENCRYPTED AMIs can be shared but corresponding CMKs used to encrypt it must also be shared.
	D. (Doubtful) Encrypting Root Volume of any EC2 instance:
		1. Create a snapshot of the EC2 instance
		2. Create a copy fo snapshot ans select the encrypt option.
		3. Create AMI from the encrypted snapshot
		4. Launch an EC2 instance from that encrypted AMI.

## EFS (Elastic File System)
- EFS is a file storage service similar to EBS
- But EFS is Elastic, meaning storage capacity grows and shrinks as per usage.
- Features:
	- Supports Network File System v4 (NFSv4) Protocol.
	- Only pay for the storage you use.
	- Can scale upto *petabytes*.
	- Can support thousands of concurrent NFS connections.
	- Data is stored across multiple AZs within a region.
	- Read after Write Consistency.
- As it is a network file storge, EFS can be mounted on multiple EC2 instances.
[https://docs.aws.amazon.com/efs/latest/ug/wt1-test.html] (Mounting EFS on any EC2 instance)

## Placement Groups
- When you launch a new EC2 instance, the EC2 service attempts to place the instance in such a way that all of your instances are spread out across underlying hardware to minimize correlated failures. 
	- You can use placement groups to influence the placement of a group of interdependent instances to meet the needs of your workload. 
- Depending on the type of workload, you can create a placement group using one of the following placement strategies:
	1. Cluster – packs instances close together inside an Availability Zone. 
		- This strategy enables workloads to achieve the low-latency network performance necessary for tightly-coupled node-to-node communication.
	2. Partition – spreads your instances across logical partitions such that groups of instances in one partition do not share the underlying hardware with groups of instances in different partitions. 
		- This strategy is typically used by large distributed and replicated workloads, such as Hadoop, Cassandra, and Kafka.
		- There is maximum of 7 partitions allowed per AZ, but number of intances per partiton is only limited by the total number of instances allowed per account
	3. Spread – strictly places instances across distinct underlying hardware to reduce correlated failures.
		- Since, there could be only one instance per partition, there is a maximum of 7 instances allowed per AZ
- There is no charge for creating a placement group.

**Placement Group Exam Tips:**
1. Cluster Placement Group cannot span multiple AZs.
	- While, Spread Placement Group can spread accross multiple AZ.
2. Name of placement group must be unique within an AWS account.
3. It is recommended that homogenous instances are placed within a placement group.
4. You can’t merge placement groups. Instead, you must terminate the instances in one placement group, and then relaunch those instances into the other placement group
5. An already created instance cannot be moved into a placement group.
6. In order to move an instance, one can create an AMI from that instance and then launch it into a placement group.
[https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/placement-groups.html] (Placement groups types and explanations)
7. To ensure that network traffic remains within the placement group, members of the placement group must address each other via their private IPv4 addresses or IPv6 addresses

