# Route 53
## DNS (Not Important for Exam)
- Converts human friendsly domain names (http://google.com) to machine IP address (http://82.123.43.1)
- IPv4 is 32 bit long and thus addresses are running out
	- IPv6 was created to solve this problem, which is 128 bit long

## Top/Second Level Domains (Not Important for Exam)
- www.google.com = .com is a top level domain
- www.wikipedia.co.uk = .uk is top level domain, .co is second level domain
- These top-level domains are registered at https://www.iana.org/domains/root/db

## Domain Registrars (Not Important for Exam)
- A registrar is an authority that can assign domain names directly under one/more top-level domains
- Top registrars include amazon, godaddy.com, 123-reg.co.uk etc.
- Uniqueness (whether a domain name that is being registered has not been registered or not) is imposed by InterNIC, a service provided by ICANN.
- Once registered, it is saved in a central database known as WhoIS database

## How DNS resolution Works (Not Important for Exam)
Browser/OS (Sends Domain Name) => 
- Resolver (maintained by our ISP) => 
- - Root Server (Managed by different organisations) => 
- Resolver =>
- - - Top Level Domain (TLD) Server (.com,.net,.org each top level domain has different Server) => 
- Resolver => 
- - - - Authoritative Name Servers (NS) (can be managed by anyone)  =>
- Resolver (Caches the IP Address) =>
Browser/OS (Caches the IP Address)

## (TERM) Apex/Root Domain (Not Important for Exam)
A domain without a first part (Sub Domain)
	- https://google.com is an apex domain
	- https://www.google.com or https://m.google.com has subdomain 'www' or 'm' 

## (TERM) SOA Record (Not Important for Exam)
- NS server is generally maintained as a Primary DB and a couple of Secondary DBs, for failover
- Every NS server maintains a SOA record
	- This record is used by NS Server to sync among the multiple NS deployments.

## (TERM) NS Records (Not Important for Exam)
- At every level of DNS resolution NS record points to the next record.
- e.g: Root Server has NS record that points to next TLD server,
	- Similarly, TLD server has NS record that points to NS server.

## TTL
- Here, we define the time period for which DNS results will be cached on resolver and on our local machines

## Alias Records (A Record)
- A resource (IP Address) can be assigned to a domain name (or subdomain), Alias Records store the actual IP Address and Domain Name to be resolved.
- Alias Records allow apex records, on left.
- Alias Records allow domain names, on right.
	
## Canonical Name (CName)
- Canonical names are like nicknames, several domains can be made to resolve to a single IP Address using CNames.
- mobile.cricinfo.com, m.cricinfo.com, web.cricinfo.com => cricinfo.com
- CNames does not allow apex records, on left.
- CNames does not allow IPAddress, on right.

- Standard Practice (Not necessary to be like this): 
	1. Create an A-Record with @ symbol for an apex domain
		(google.com) @ => 31.21.12.3
	2. Create CName records that then map subdomains to the above A-Record
		(www.google.com) => @ (this symbol would automatically refer to the apex record set above)

## Common DNS Types (Not Important for Exam)
1. SOA records
	- Automatically generated when a domain is bought from amazon
	- Also, created when a publicly hosted zone is created (For migrating domain name from other provider to amazon)
2. NS Records
	- Similar to SOA, Automatically generated when a domain is bought from amazon or a publicly hosted zone is created
3. A Records
4. CNAMES
5. MX Records (For Emails)
6. PTR Records (For Reverse DNS Lookups)

**Domain names can be bought directly from AWS. Domain registration can take upto 3 days**

## What is Route 53?
- AWS's Route53 provides 3 main functions:
	1. Domain Registrations
		- allows you to register domain names
	2. DNS Reolution Service
		- resolving domain names to ip addresses.
		- can route internet traffic to CloudFront, Elastic Beanstalk, ELB, or S3
		- There’s no charge for DNS queries to these resources
	3. Health Checking
		- can monitor the health of resources such as web and email servers.
		- sends automated requests over the Internet to the application to verify that it’s reachable, available, and functional
		- CloudWatch alarms can be configured for the health checks to send notification when a resource becomes unavailable.
		- can be configured to route Internet traffic away from resources that are unavailable
- Route 53 runs on Edge Locations

## Route 53 Exam Tips:
1. ELB's do not have pre-defined IPv4 addresses, you resolve to them using DNS name.
2. Given a choice to choose CName over A-Record in AWS exams, aways choose A-Record.

## Health checks
- Route53 health checks take ipaddress, hostaname, port and path to ping.
- It will check the path at fixed interval and decide whether the route is healthy or not 
- While setting up any Routes, a health check can be selected

## Route53 Routing Policies
1. Simple Routing
2. Weighted Routing
3. Latency-Based Routing
4. Failover Routing
5. Geolocation Routing
6. Geoproximity Routing
7. Multivalue Answer Routing

## Simple Routing
- A single record (Domain) can point to multiple IP Addresses
- Route 53 sends all values to the user in a random order.
- A health check cannot be assigned.

## Weighted Routing 
- Allows you to split your traffic based on weights
- e.g: 10% of traffic can go to US-EAST1 and 90% can go to EU-WEST1
- A single record can include one or more IP Addresses.
- Each record also includes particular weights.

## Latency Based Routing
- Routes traffic based on the lowest latency experienced by user
	- e.g: Indian users would be routed to Mumbai Region if we have a server hosted there. But it is not strictly maintained, at any point in time a Japan Server might provide lower latency than Mumbai, then the user will be routed there.

## Failover Routing
- While using Failover routing, 2 servers could be maintaines such that when primary server fails we could switch over to secondary server

## Geolocation Routing
- One can route a user based on routes. Such that all european users will be routed to servers that has been configured to serve european customers (Language, currency symbol etc.)

## Geoproximity Routing (Only accessible using Traffic Flow)
- One can choose routing based on location of user as well as resources
- Also, one can add bias (similar to weights) so that a particular user will be preferabbly sent over to a particular region
- Traffic Flow provides a graph based GUI that has logic to govern how the traffic will flow using Route53
- All routing policies can be defined using Traffic Flow.

## Multivalue Answer Policy
- Multivalue Answer policy is similar to Simple routing, it just differs in the feature that allows you to set health checks.
- Simple Routing policy does not allow health checks, so multi value was introuced.

