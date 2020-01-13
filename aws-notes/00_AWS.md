# AWS

## Cloud Types

1. Public : A cloud managed by an organization and open to use by the general public.
2. Private : A cloud that virtualizes and distributes the IT infrastructure for a sin-gle organization.
3. Hybrid : A mixture of a public and a private cloud.AWS is a public cloud.

## Cloud Classifications

1. Infrastructure as a service (IaaS)—Offers fundamental resources like computing,storage,  and  networking  capabilities,  using  virtual  machines  such  as  AmazonEC2, Google Compute Engine, and Microsoft Azure.
2. Platform as a service (PaaS)—Provides platforms to deploy custom applications tothe cloud, such as AWS Elastic Beanstalk, Google App Engine, and Heroku.
3. Software  as  a  service  (SaaS)—Combines  infrastructure  and  software  running  inthe cloud, including office applications like Amazon WorkSpaces, Google Appsfor Work, and Microsoft Office 365.

## Scenarios

### Hosting a Web Shop

Dynamic content : DNS - LOADBALANCER - WEB SERVER - DATABASE</br>
Static Content : DNS - CDN - S3

### Running an application in your private network

Corporate Network - (via) VPN Gateway - *AWS Private Subnet*</br>
*AWS Private Subnet* - NAT Instance - Internet Gateway - Internet</br>
*AWS Private Subnet* - SQL Database

### Implementing a highly available system

Internet facing Loadbalancer - *EC2 Instance 1* & *EC2 Instance 2*</br>
*EC2 Instance 1* - Database (master)</br>
*EC2 Instance 2* - Database (standby)

## Costing

1. Based on minutes or hours of usage 
    - Loadbalancer
    - EC2 machine
    - Database
2. Based on traffic : Traffic is measured in gigabytes or in number of requests
    - Loadbalancer
    - DNS
    - CDN
    - S3
3. Based on storage usage : Usage can be measured by capacity (for example, 50 GBvolume no matter how much you use) or real usage (such as 2.3 GB used)
    - Database
    - S3

## AWS Resources

- Accessing and Modifying AWS Resources

    1. Web-based UI, like the Management Console
    2. A command-line interface (CLI)
    3. programmatically via  an  SDK
    4. Blueprints (Cloud Formation Templates)

- Getting started

    1. Create AWS Account
    2. Create Key Pair (for logging into ec2 instances)
    3. Create a Billing Alarm
