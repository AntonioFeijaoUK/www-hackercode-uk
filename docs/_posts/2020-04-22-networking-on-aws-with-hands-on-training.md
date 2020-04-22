---
title: "Networking on AWS with hands on training"
date: 2020-04-22
catergories:
    - AWS
    - Networking
tags:
    - networking
    - linux
    - aws
---

Networking on [AWS](https://aws.amazon.com) with hands on training

---

## Agenda Day 1 - start easy, cover the basics

**08:00 - 08:10** ( 10m) - get ready, set, ready, go

    - Account Region
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
    - Central Security VPC
    - TGW
    - VPN
    - VGW
    - DXGW


---

## Exercise 101

![vpc igw ngw](/images/vpc-igw-ngw.png)


## Exercise 301

* [Networking Exercise 301](https://github.com/ajitsinghr/networkworkshop)

[Networking Exercise 301 Template](/assets/net-exercise-301.yaml)

![Networking Exercise 301](/images/net-exercise-301.png)


---

## Questions

---

## Resources

### Online training

- <https://www.aws.training/>
- <https://cloudacademy.com/>
- <https://acloud.guru/>
- <https://www.antoniocloud.com/training/aws/education/aws-online-training/>

- (Free access until end of April) - <https://app.pluralsight.com/library/courses/advanced-networking-aws/table-of-contents>

### SCP Policies

<https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_example-scps.html>


### AWS EC2 sample userdata

lunch EC2 with webserver - <https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html>

EC2 metadata - <https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-identity-documents.html>

<script src="https://gist.github.com/AntonioFeijaoUK/d8533a71e5ecff2971f6859a7be426da.js"></script>
