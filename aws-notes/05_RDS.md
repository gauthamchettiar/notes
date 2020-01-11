# RDS - Relational Database Service

- Relational DB supported by AWS:
	1. SQL Server
	2. Oracle
	3. MySQL Server
	4. PostgreSQL
	5. Aurora DB (Amazon's MySQL based DB)
	6. MariaDB
- Every RDS when created has an endpoint assigned, one can connect to the database using this endpoint, username and password (that is set by user during creation) 

## Database Usecases

- OLTP: SQL Server, Oracle, MySQL Server, PostgreSQL, Aurora DB and MariaDB
- OLAP (Data Warehousing / Business Intelligence Solution): Redshift
- NoSQL: DynamoDB

## RDS Exam Tips

1. RDS Runs on virtual machines
	- One cannot log into these virtual machines.
2. Thus one cannot apply security patches to underlying OS and Database.
	- However Amazon takes care of these updates.
3. RDS is NOT SERVERLESS
	- But, Aurora Serverless IS SERVERLESS
4. Any MySQL Database can be migrated to Aurora DB.

## RDS Backups

- Backups in RDS can be performed in two ways:
	1. Automated Backups
		- As name suggests when automated backups are enabled a daily backup of database is taken.
		- Along with a daily backup the entire day's transaction logs are stored.
			- This allows AWS to perform recovery upto the second.
		- A retention period from 1 to upto 35 days can be selected.
		- Automated backups are stored in S3, you get free storage space equal to your Database size.
		- The time of day when the backup must be taken can be selected by us (this time is called maintenance window)
			- During this period a slight performance hit could be expected.
		- Automated backups are enabled by default
		- Once the original DB is deleted we are provided with an option to either delete the backups or keep them.
	2. Database Snapshots
		- They are user initiated backups.
		- Similar to EC2 snapshots, RDS snapshots are not related to the parent database
			- They persist even after the original DB is deleted.

### Restoring backups

- Whenever a backup is restored using both the above backup methods, the new DB will be a new RDS instance with new DNS endpoint.

### Encryption At Rest

- It is supported for all OLTP databases.
- Once encrypted all the data stored in the underlying storage is encrypted, along with the automated backups, snapshots and read replicas (explained later)


## RDS Multi-AZ Feature

- Multi-AZ allows to have an exact copy of your database on another Availability Zone, called secondary/standby DB.
- With having Multi-AZ enabled, a standby database is in place. 
	- When the primary DB fails, the secondary DB comes to serves instead.
- Supported for *SQL Server, Oracle, MySQL Server, PostgreSQL, and MariaDB* (Aurora DB by architecture is fault tolerant hence doesn't support Multi-AZ feature seperately)
- Multi-AZ feature is used mostly for DISTASTER RECOVERY
- Even when the database is switched after a failover, endpoint does not change.
- It is useful in scenarios like,
	1. Planned maintenance
	2. DB instance Failure
	3. Availability Zone Failure
- A failover can be forced by rebooting an RDS instance.

## RDS Read Replica

- With read replica enabled, one can create multiple replicas of primary database (upto 5) and allow users to read the database from the created database instead.
- Unlike Multi-AZ feature, all the read-replicas are active at the same time and serve under different endpoints.
- Supported for *MySQL Server, PostgreSQL, Aurora DB and MariaDB*
- *Usecase*: Scaling, for read heavy applications. Increases performance, not used for disaster recovery.
- Even though there is a limit of upto 5 read replicas one can have read replicas of read replicas.
- A read replica can have it's own multi-AZ
- A multi-AZ enabled Database can have read replica enabled.
- Read replicas can be promoted to their own databases, in which case it will no longer replicate the source database.
- Read replica can be created in a seperate region.

# Amazon DynamoDB

- Amazon's NoSQL solution that provides very fast read/writes, a single digit millisecond latency at any scale.
- Database is completely managed by AWS and supports both, 
	1. Document &
	2. Key-Value Data models
- DynamoDB is stored on SSD Storage.
- DynamoDB is pread accross 3 AZs (Thus service is not supported in regions with less than 3 AZs)
- Provides 2 type of services:
	1. Eventual Consistency Reads (Default)
		- Provides the best read performance
		- There is a second of latency for the written data to fully reflect on the database
		- Used in scenarios where there is high number of reads.
	2. Strongly Consistent Reads
		- Returns only the data that has been recently written.
		- Read performance is a bit slowed down, but in return you get immediate consistency.

# Amazon Redshift

- A datawarehousing (BI) tool that provides fast petabyte scale service.
- Starts for as small as 0.25$ per hour upto $1000 per terabyte per year. (for ondemand basis)
- RedShift can be configured as follows:
	1. Single Node (160GB)
	2. Multi Node:
		- Leader Node (Manages Client connections and recieved queries)
		- Compute Node (Store data and perform computations)
			- Upto 128 Nodes
- Automated and Advance Compression:
	- Redshidt stores data as a column data store (instead of traditional row-based data store).
	- This allows redshift to compress the data more efficiently.
	- When any structured or unstructured data is loaded into a table in redshift, it automatically samples the data and chooses the appropriate compression technique.
- Backups:
	- Enabled by default with retention period of 1 day.
	- Maximum retention period of 35 days.
	- Redshift always tries to maintain 3 replicas of data,
		1. The original
		2. Replica on compute nodes
		3. Backup in Amazon S3
	- Backup can be made on S3 from nother region for Disaster Recovery.
- Pricing:
	1. Compute Node Hours (running 3 node for a day will incur 3x24hours=72 instance hours)
	2. For Backups
	3. Data Trasfer (within VPC)
- Security:
	1. Encrypted in transit using SSL
	2. Encrypted at rest using AES-256 (Using red-shift's key) OR
		- Manage your keys using HSM
		- Mnage your keys using AWS KMS
- Availability:
	- Currently only available in 1 AZ
	- Can restore snapshots/backups to new AZs in event of outage

# Amazon Aurora

- Aurora is a MySQL compatible, relational DB engine
- It is said to provide 5 times better performance than MySQL at 1/10 of the commercial DB price.
- Start with 10GB, increments at 10GB upto 64TB
- Compute resources can scale upto 32vCPUs / 244GB of memory
- 2 Copies of data is maintained per availability zone, data is maintained atleast across 3 AZs (A minimum of 6 copies)
- Scaling:
	- Handles loss of upto 2 copies for write availability
	- Handles loss of upto 3 copies for read availability
	- Self healing: Disks are continously scanned for errors and repaired automatically
- Replicas (Read Replicas in MySQL)
	- Aurora Replicas (Upto 15)
	- MySQL Read Replicas (Upto 5)
- Backups:
	- Automated backups are always enabled.
		- Unlike MySQL backups donot affect database performance
	- Manual Snapshots
		- Also doesn't affect performance
		- *Aurora snapshots can be shared with other accounts*
	
# Elasticache

- On Cloud, in memory, cache store, that is easy to deploy, operate and scale.
- It improves performance by allowing to cache most frequently used data and retrieve them quickly
	- e.g: Cache top 10 search results of amazon
- Supports 2 types f open source, in memory caching engines:
	1. Memcached
		- For simple use cases
		- For usecases requiring multi threaded use and ability to scale horizontally
	2. Redis
		- Has Multi-AZ functionality
		- Backups and restore is possible