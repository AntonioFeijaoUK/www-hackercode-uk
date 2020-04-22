---
title: "Networking on AWS with hands on training"
toc: true
date: 2020-04-22
categories:
    - AWS
    - Networking
tags:
    - networking
    - linux
    - aws
---

{{ content | toc }}

Networking on [AWS](https://aws.amazon.com) with hands on training. This training session is delivered across 2 days. During **Day 1**, we will cover the basics [Why AWS](https://aws.amazon.com/products/), [What is Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html), [architectural best practices](https://aws.amazon.com/architecture/well-architected/), and lot more topics, and of course, `hands on`! **Day 2** will be for advanced users where we will discuss [AWS Transit Gateway](https://aws.amazon.com/transit-gateway/) and other [Networking and Content Delivery on AWS](https://aws.amazon.com/products/networking/) solutions! ![TGW](https://d2908q01vomqb2.cloudfront.net/5b384ce32d8cdef02bc3a139d4cac0a22bb029e8/2019/10/01/TransitGateway2.png)

---

## Useful links for the day

* [https://dashboard.eventengine.run/login](https://dashboard.eventengine.run/login)
> User `Free tier` option
> North Virginia


---

## Agenda Day 1 - AWS EC2 and VPC fundamentals

**08:00 - 08:10** ( 10m) - get ready, set, ready, go

    - Account
    - Region 
    - User Access
    - MFA?
    - users - roles - policies - permissions

**08:10 - 09:10** ( 60m ) - Covering basics of Networking on AWS

    - VPC - IP Range
    - Security Groups (port 80 -  0.0.0.0/0)
    - NALC
    - Route Tables
    - Availability Zones
    - Regions
    - ALB / ELB / NLB
    - Elastic IPs

**09:10 - 09:20** (10 min) - Quiz

**09:20 - 09:30** (10m) - break

**09:30 - 12:00** (2h:30m) - Exercise 1, 2 and 3

**12:00 - 13:00** - lunch break

**13:00 - 13:45** - Elastic Beanstalk with “Configure Options”

    - Create environment
    - Explore the options

**13:45 - 14:00** - break

**14:00 - 16:00** - Networking level 301

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

## Exercise level 101

![vpc igw ngw](/images/vpc-igw-ngw.png)

```
*Exercise 1 - Default VPC and Wizard VPC*

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

## Exercise level 102

```
*Exercise 2 - Public instance*

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

## Exercise level 103

```
*Exercise 3 - Private instance*

* Private Subnet
* NAT Gateway vs NAT Instance
* wordpress, webpage, database - "AWS Elastic Beanstalk" 

```



---

## Exercise level 301

[Networking Exercise 301 Template](/assets/net-exercise-301.yaml)

![Networking Exercise 301](/images/net-exercise-301.png)

Source [Networking Exercise 301](https://github.com/ajitsinghr/networkworkshop)



---

## Agenda Day 2 - start easy, cover the basics

- VGW
- DX
- DXGW - Direct-Connect Gateway
- Access S3, through internal services service endpoints
- Op-prem connections, hybrid (focus on internal connection)
   
- SD-WAN

- Write a script - dealing with some automation

- CloudFormation
- CloudFormation template - <https://github.com/awslabs/aws-cloudformation-templates>

- Route53 

- Route53 Resolver integration with internal DNS, integration with Active Directory

- Lambda

- internal LoadBalancer (Hand-on)

- Gateway endpoints
- PrivateLink
- NLB - Networking LoadBalancer
- Global Accelerator


---

## Questions Day 1

- "How do I check my the availability zone id?"
    - On the console, you can check in VPC, then subnets.
    - "An Availability Zone in my account doesn't map to the same location as an Availability Zone with the same name in a different account. However, you can use AZ IDs to map Availability Zones across accounts"
    - see this link to find more <https://aws.amazon.com/premiumsupport/knowledge-center/vpc-map-cross-account-availability-zones/>


---

## Resources

### Sample of networking solutions

- "Creating a single internet exit point from multiple VPCs Using AWS Transit Gateway"
    - <https://aws.amazon.com/blogs/networking-and-content-delivery/creating-a-single-internet-exit-point-from-multiple-vpcs-using-aws-transit-gateway/>



### Online training

- <https://www.aws.training/>
- <https://cloudacademy.com/>
- <https://acloud.guru/>
- <https://www.antoniocloud.com/training/aws/education/aws-online-training/>

- (Free access until end of April) - <https://app.pluralsight.com/library/courses/advanced-networking-aws/table-of-contents>

### Service control policies (SCPs)

- <https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_example-scps.html>


### AWS EC2 sample script for userdata

- "Running Commands on Your Linux Instance at Launch"
    - <https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html>

- "Instance metadata and user data"
    - <https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-identity-documents.html>

- Sample userdata shell script that install a webserver with basic information about the instance. Can you launch it with ALB?

<script src="https://gist.github.com/AntonioFeijaoUK/d8533a71e5ecff2971f6859a7be426da.js"></script>

---

## Feedback

> Did you learn something new today?
> How was the presenter?
> Was it good use of your time?
> Thank you for your constructive feedback, it will make a difference on future training sessions.

### [LINK to feedback **Day 1**](https://survey.immersionday.com/s00L_23ZR)

### [LINK to feedback **Day 2**](https://survey.immersionday.com/WBsY_hqWg)

---

## Follow up

Feel free to connect on [LinkedIn](https://www.linkedin.com/in/antoniofeijaouk/)

