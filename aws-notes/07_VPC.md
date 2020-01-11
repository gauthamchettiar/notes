# VPC - Virtual Private Cloud

- VPC is like a virtual data center
- VPC enables you to provision an logically isolated section in AWS
- One can set his own IP address range, creation of subnets and configure route tables and network gateways
- Security in VPC can be configured with:
	- Route Tables
	- Internet N/W Gateway
- Features:
	1. Instances can be launched into any Subnet
	2. Custom IP Address ranges can be choosen for each subnet
	3. Route tables can be configured between subnets
	4. IGW can be attached to a VPC, giving it access to the internet.
	5. Per instance can be restricted by Security Groups

## VPC Creation:

1. Name
2. CIDR Block (User Provided)*
3. IPv6 (Yes/No)
4. Tenancy (Default/Dedicated)

### Components created along with VPC

1. Default (Main) Route Table  (Allow communication within VPC)
2. Network ACL (Allows all inb ound and outbound)
3. Default Security Group (Allows all Inbound from same security group and Allows all Outbound)

### Components that needs to be created by user

1. Subnets
2. Internet Gateway

## Components in VPC

1. IGWs (Virtual Private Gateways)
2. Route Tables
3. Network Access Control Lists
4. Subnets
5. Security Groups

# Internet Gateway

- A VPC can be connected to an Internet gateway, which acts as a gateway to internet.
- A VPC can be connected to only one VPC and vice versa.
- For a subnet to be connected to internet (ie: creating a Public Subnet) a Route to IGW should be added to Route table.

# Subnets

- Inside a VPC several subnets can be created.
- A Subnet can be either Public facing or Private facing:
	1. Public Facing:
		- Any subnet associated with Internet Gateway are public facing
		- Websites that could be accessed by users must be on public subnet
	2. Private Facing:	
		- Any subnet w/o any association to Internet Gateway are private facing
		- Private subnets are apt for databases that should not be accessible from internet
- Security in Subnets can be configured with:
	- Security Groups
	- Network Access Control List
- A subnet mustalways be associated with a Route Table
- A subnet can only be associated with one AZ
	- though an AZ can consist of multiple Subnets.
- *Amazon Reserves 5 IP address from every subnet (4 starting IP address and 1 last IP address)*

## Subnet creation:

1. Name
2. VPC *
3. AZ (If not selected by user, a random AZ will be assigned)
4. CIDR Range *
5. IPv6 Required? (Yes/No)

# Private CIDR (IP) ranges 

- 10.0.0.0 to 10.255.255.255 (10/8)
- 172.16.0.0 to 172.31.255.255 (172.16/12)
- 192.168.0.0 to 192.168.255.255 (192.168/16)

# VPN

- A Hardware VPN connection can be setup between an organization's data center and AWS Cloud
- This connection will be charged per GB of data transferred

# Default VPC

- When an account is created default VPC is created along with it
- Each AZ in a region has a subnet associated with it
- All subnets in Default VPC have a route out to internet (has IGW connected)
- All EC2 instances in Default VPC has both a public and private IP address
- *When an instance is launched without specifying a subnet, it will be launched into the default VPC*

# VPC Peering

- When VPC peering is enabled, Any 2 VPC can communicate with each other as if they were in same VPC
- Since VPC is connected using direct network routing, each instance can communicate using private IP addresses
- VPC peering can be setup between VPCs in different accounts as well

```
     D
     |
B <- A -> C
     |
	 E
```

- In above configuration, VPC A can be peered to VPC B,C,D and E in a star configuration.
- There is no transitive peering, In above configuration VPC B cannot talk to VPC D. 
	- For them to communicate they need to be peered seperately

# Internet Access to Instances inside a VPC

1. Launching your VPC in a default VPC
	- All instances launched within a default VPC has a private as well as public IPv4 address.
	- All instances can communicate with internet since they are by default connected to IGW
2. Launching your VPC in a non-default (custom) Public VPC
	- Any instance within a public VPC must have a public IP address in order for it to access the internet.
		- Public IP Address can be assigned by changing Subnet's public IP adress setting (Can be done post creation from Actions -> Modify Auto-Assign IP Settings)
		- Or Assign IP Address to individual instance during instance creation
	- Any instance in Private Subnet can be allowed an access to internet without a public IP using NAT instances/Gateways
		- In this case only (stateful) egress traffic to internet is allowed
	
# Route Tables

- A route table contains a set of rules, called routes, that are used to determine where network traffic is directed
- Every subnet must be associated with a Route Table
- A subnet can only be associated with one route table at a time
	- but you can associate multiple subnets with the same route table
	- e.g: 	1 RT to Multiple Subnets
			1 Subnet to 1 RT
- Every route table (whether main or custom) has predefined routes allowing communication within VPC
- **Main Route Table:**
	- Main RT is created along with the VPC
	- Any Subnet that is not explicitly associated with a Route Table will be implicitly associated with Main Route Table
- **Custom Route Tables:**
	- Any subnet needs to be explicitly associated with custom RTs

## Route Table Creation

1. Name
2. VPC *

## Adding Route

1. Destination IP *
2. Target (Could be an instance, IGW, NAT G/W, Virtual G/W, etc.) *

# Network ACL

- Network ACL is stateless security protocol that can Allow/Deny particular IP address on any port
- Any newly created NACL will have All Ingress and Egress Deny
- Rules are applied on the basis of rule number
	- Lower Number = Higher Priority
- A subnet can only be assigned with one NACL
	- Multiple subnets can be assigned to a single NACL
	- e.g: 	1 NACL to Multiple Subnets
			1 Subnet to 1 NACL
- It is recommended to keep all deny rules on higher priority than allow rules
- **Default NACL**
	- Created when any new VPC is created
	- Allows all inbound and outbound Traffic
	- Any new subnet created will be assigned with default NACL
- **Custom NACL**
	- Any NACL created by user is a custom NACL.
	- Denies all inbound and outbound traffic.

## NACL Creation

1. Name
2. VPC *

## Adding Rule to NACL

Select Either Inbound Tab or Outbound Tab

1. Rule Number (Between 1 to 32766)
2. Type (Has some predefined protocols and ports)
3. Protocols (Is enabled if type is custom)
4. Port Range
5. Source IP
6. Allow/Deny

# NAT

- **NAT Instances**
	- NAT Instances are user managed instances that is available as an Amazon AMI
	- NAT Instances are launched in a **public subnet**
	- For NAT Instances to work one has to **disable source and destination checks on that instance**
	- A Route table is set in private subnet that connects instances that require internet access with NAT instance.
	- NAT is a physical instance that has limitations and bottlenecks pertaining to instance type. 
	- Since NAT instances are user managed, one has to manually setup autoscalling and failover mechanisms
	- NAT Instances are setup behind a security group
	
- **NAT Gateways**
	- NAT gateways are a better alternative to NAT instances.
	- It is a fully managed service that is redundant (within AZ) and highly available.
	- Provides speed from 5GBPS to 25GBPS
	- NAT gateway is NOT setup behind a Security Group
	- NAT Gateways are assigned a public Elastic IP on creation
	- **NAT Gateway Creation**
		1. Subnet *
		2. Elastic IP Allocation ID *
	- **For Multi AZ Deployments**
		- It is recommended to create NAT gateways in each AZ and associate instances from a particular IP with NAT gateway of the same AZ.
		- This way if an AZ goes down, instances from other AZs won't be affected


# VPC Flow Logs

- Logs all the accepted and rejected requests made on the VPC
- Can be logged to Cloudwatch or to an S3 bucket

**Exam Tips:**

- Flow logs cannot be enabled on VPC peered network of different account.
	- However for VPC peered account within same account it should be possible
- Flow Logs cannot be tagged
- Once a flow log is created, t's configuration cannot be changed
- **Traffic data that cannot be monitored using flow logs**
	- Traffic generated by instances when they contact Amazon's DNS service
		- for self managed DNS service, it will be logged
	- Traffic from any windows instance
	- Traffic to/from 169.254.169.254 for instance metadata
	- DHCP traffic
	- Traffic to the reserved IP Address for default VPC router	

## VPC Flow Log Creation

1. Resources (ID of VPC) *
2. Filter (All/Rejected/Accepted)*
3. Destination (Send to Cloudwatch/S3 Bucket)
4. Destination Log Group / S3 Destination Bucket (Depending on where to send the logs)
5. IAM Role (IAM role with permission to push logs to cloudwatch)

# Bastion Host

- A host using which instances in private network can be accessed.
- In order to connect to private network instances, bastion hosts will be contacted which will redirect traffic to the desired instance.
- Bastion hosts will be hosted in public subnet.

# Direct Connect

- Connects Corporate network directly to AWS network
- A high speed connection is setup as follows:
	AWS Network -- (AWS Backbone network) -- DX Location (AWS Cage and Customer Cage) -- (Customer provided link) -- Customer Network
- Is used by organizations requiring high throughput (network traffic) to AWS service without going through internet
	- is also used by organisations if they need relaible and secure connection


# VPC Endpoint

- For AWS services that require internet access (S3 buckets) a VPC endpoint can be created between an instance and that particular service.
- The traffic would then only go through AWS network without requiring instance to have internet access (using NAT instance/gateway or IGW)
- VPC Endpoint can be created in 2 ways:
	1. Interface Endpoint
	2. Gateway Endpoint
- **Interface Endpoint**
	- An interface endpoint is an elastic network interface with a private IP address that serves as an entry point for traffic destined to a supported service. 
	- Supported for vast majority of AWS services
- **Gateway Endpoint**
	- A gateway endpoint is a gateway that is a target for a specified route in your route table, used for traffic destined to a supported AWS service. 
	- Supported for Amazon S3 and DynamoDB