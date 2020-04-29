---
title: "Networking on AWS with hands on training"
layout: post
toc: true
author: "Antonio Feijao UK"
date: 2020-04-22
categories:
    - AWS
    - Networking
tags:
    - networking
    - linux
    - aws
---

Networking on [AWS](https://aws.amazon.com) with hands on training. This training session is delivered across 2 days. During **Day 1**, we will cover the basics [Why AWS](https://aws.amazon.com/products/), [What is Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html), [architectural best practices](https://aws.amazon.com/architecture/well-architected/), and lot more topics, and of course, `hands on`! **Day 2** will be for advanced users where we will discuss [AWS Transit Gateway](https://aws.amazon.com/transit-gateway/) and other [Networking and Content Delivery on AWS](https://aws.amazon.com/products/networking/) solutions!

{:toc}

## Useful links for the day

- [Account Login](https://aws.amazon.com)

    User `Free tier` option
    North Virginia

- [Networking Whiteboard](https://awwapp.com/b/ubnyz93a8ilwh/)

---

## Day 1

### Agenda - AWS, EC2, VPC fundamentals, Networking fundamentals on AWS

- AWS Account
- Region 
- User Access
- MFA
- Users, Servers and permissions
  - IAM, Users
  - Roles, Servers, Instances, Serviecs
  - Policies, Permissions
  - SSO

- VPC - IP Range
  - Security Groups (port 80 -  0.0.0.0/0)
  - NALC
  - Route Tables
  - Availability Zones
  - Regions
  - ALB / ELB / NLB
   - Elastic IPs

- Quiz

- Exercise 1, 2 and 3

- Elastic Beanstalk with “Configure Options”
  - Create environment
  - Explore the options
  - Review Autoscaling

- Intro to Networking solutions on AWS
  - VPC-Endpoints
  - PrivateLink
  - Shared VPC
  - Solution `Central Security VPC`
  - AWS Transit Gateway (TGW)
  - VPN
  - VGW
  - DX
  - DXGW

---

### Exercise 1 - EC2 instance in the Default VPC

- Luanch an EC2 instance in the Default VPC
- User the `userdata` script provided below
- Open a browser page with the IP address of the EC2 instance

---

### Exercise 2 - EC2 instance in private subnet

- VPC Wizards
  - Public Subnet
  - Private Subnet

- Luanch Instance in private subnet
  - EC2 Amazon Linux v2

![vpc igw ngw](/images/vpc-igw-ngw.png)

```text

* Check default VPC
    * ip range
    * DHCP Options set
* Created and review your own VPC with *VPC Wizard*
    * Allocate an “*Elastic IP*” or “*NAT instance*”
    * VPC Name
    * Public Subnet
        * IP range
        * Availability Zone
        * Public Subnet name
    * Private Subnet
        * IP range
        * Availability Zone
        * Public Subnet name
    * Use “*NAT instance*”
    * delete...
```

---

### Exercise 3 -  Connect to instance in private subnet without ssh keys

- Create SSM ROLE
  - attach SSM-Role to the instance in private subnet
  - Connect view SSM
  
- Additional challenge
  - Security Groups allowing only necessary ports
  - Can you add PrivateLink EndPoints for SSM

```text

* Create ROLE “*AmazonEC2RoleforSSM*”
* Create Manually a VPC
* Public Subnet
* Routing Table
* Internet Gateway (IGW)
* Security Groups
* Start and instance (web-server)
    * userdata <--- see script in resources below
```

---

### Exercise 4

- Using `AWS Elastic Beanstalk`

- Create, investigate
- Autoscaling group
- Launch configuration

```text

* Private Subnet
* NAT Gateway vs NAT Instance
* Wordpress, Web-page, Database - "AWS Elastic Beanstalk" 

```

---

## Day 2

### Agenda - Advanced networking solutions on AWS TGW, VGW, NatGW, DX, DXGW and more

- VGW
- DX
- DXGW - Direct-Connect Gateway
- Access S3, through internal services service endpoints
- Op-prem connections, hybrid (focus on internal connection)

- SD-WAN

- Write a script - dealing with some automation

- CloudFormation
- CloudFormation template by `awslabs` - <https://github.com/awslabs/aws-cloudformation-templates>

- Route53

- Route53 Resolver integration with internal DNS, integration with Active Directory

- Lambda

- internal LoadBalancer (Hand-on)

- Gateway endpoints
- PrivateLink
- NLB - Networking LoadBalancer
- Global Accelerator

---

### Exercise 1

- Create a TGW

- Create VPC-192-168 (`192.168.0.0/24`)
  - Public Subnet
  - Private Subnet
  
- Create VPC-10-0 (`10.0.0.0/24`)
  - Public Subnet
  - Private Subnet

- Create SSM-ROLE for EC2

- Launch 2 instances on each private subnet, attach the SSM-ROLE
  - Instance private subnet on VPC-192-168
  - Instance private subnet on VPC-10-0

- **check point** - can you access both instances using SSM?

- TGW Route tables
- TGW Associations
- Return route on VPC-192-168 route table
- Return route on VPC-10-10 route table

- Can you `ping` on both directions?

- Can you `curl` on both directions? (with python3 package you can start a simple webserver `python3 -m http.server 80`

- `iperf` ?

- `curl ipinfo.io/json`, `ifconfig`, `sudo netstat -pant`

- Additional challenge
  - Security Groups allowing only necessary ports
  - Can you add PrivateLink EndPoints for SSM

- sample of [Cloudformation](https://aws.amazon.com/cloudformation/) template code for the networking diagram below

[Networking Exercise 301 Template](/assets/net-exercise-301.yaml)

![Networking Exercise 301](/images/net-exercise-301.png)

Source [Networking Exercise 301](https://github.com/ajitsinghr/networkworkshop)

---

## Questions that come up during the training

- "How do I check my the availability zone id?"
  - On the console, you can check in VPC, then subnets.
  - "An Availability Zone in my account doesn't map to the same location as an Availability Zone with the same name in a different account. However, you can use AZ IDs to map Availability Zones across accounts"
  - see this link to find more <https://aws.amazon.com/premiumsupport/knowledge-center/vpc-map-cross-account-availability-zones/>

---

## Resources

### Online training

- <https://www.aws.training/>
- <https://cloudacademy.com/>
- <https://acloud.guru/>
- <https://www.antoniocloud.com/training/aws/education/aws-online-training/>

- (Free access until end of April) - <https://app.pluralsight.com/library/courses/advanced-networking-aws/table-of-contents>

### Youtube videos

- <https://youtu.be/awrdICiS6ug> - "Advanced Architectures with AWS Transit Gateway - AWS Online Tech Talks"

- <https://youtu.be/9Nikqn_02Oc> - "AWS re:Invent 2019: [REPEAT 1] AWS Transit Gateway reference architectures for many VPCs (NET406-R1)"

- Reference network architecture (net406)

![reference network architecture net406](/images/reference-network-architecture-net406.png)

- [AWS Online Tech Talks' Youtube channel](https://www.youtube.com/channel/UCT-nPlVzJI-ccQXlxjSvJmw)

- [Amazon Web Services' Youtube channel](https://www.youtube.com/user/AmazonWebServices/featured)

- [AWS Knowledge Center](https://aws.amazon.com/premiumsupport/knowledge-center/)

---

### Service control policies (SCPs)

- <https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_example-scps.html>

---

### AWS EC2 sample script for userdata

- "Running Commands on Your Linux Instance at Launch"
  - <https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html>

- "Instance metadata and user data"
  - <https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-identity-documents.html>

- Sample userdata shell script that install a webserver with basic information about the instance. Can you launch it with ALB?

<script src="https://gist.github.com/AntonioFeijaoUK/d8533a71e5ecff2971f6859a7be426da.js"></script>

---

### Blog posts

- Sample of an architecture with single internet exit point from multiple VPCs Using AWS Transit Gateway.

![TGW](https://d2908q01vomqb2.cloudfront.net/5b384ce32d8cdef02bc3a139d4cac0a22bb029e8/2019/10/01/TransitGateway2.png)

> source <https://aws.amazon.com/blogs/networking-and-content-delivery/creating-a-single-internet-exit-point-from-multiple-vpcs-using-aws-transit-gateway/>

---

## Feedback

> Did you learn something new today?
> How was the presenter?
> Was it good use of your time?
> Thank you for your constructive feedback, it will make a difference on future training sessions.

### Thank you for your feedback

---

## Follow up

Feel free to connect on [Antonio Feijao UK's LinkedIn](https://www.linkedin.com/in/antoniofeijaouk/)
