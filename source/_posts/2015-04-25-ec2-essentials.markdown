---
layout: post
title: "EC2 Notes"
date: 2015-04-25 12:27:57 +1000
comments: true
categories: [EC2 , Amazon Architecture Certificate , Lambda ,IAM,Placement Group]
---

Some Notes about Elastic compute cloud , you will find it useful if you want to be prepare for AWS architecture certificate.

* Elastic Compute Cloud provides re-sizable compute capacity in cloud.
* It can be scale up or down as computing requirements changed.
* EC2 Options: Free Tier, On Demand , Reserved and Spot.
* Data Stored on a local instance storage will be deleted when you stop the instance.
* EBS backed storage will persist independently of the life of the instance
* EBS volume can be attached to only One instance at a time.
* Types of storage backed by EBS:
	-	General purpose SSD , 99.999% availability, 
	-	Provisioned IOPS SSD, Designed for IO intensive applications
	-	Magnetic, Low cost , Suitable for infrequent accessed data
* Identity Access Management (IAM) roles can not be assigned to an EC2 instance after the instance has been created.
* You can not add or change roles to an existing EC2 instance but you can change the permissions of a assigned role.
* EC2 placement group properties:
	- 	group of logical groups of instances in only one singe AZ
	-	Are low latency ; has 10GBbps network throughput.
	-	The name you choose for placement group should be unique in AWS account
	-	Same size instances recommended to be put in a group
	-	placement groups cannot be merged.
	-	you can add existing instances to your placement group by creating AMI form that instance and then launch it to your placement group
* Lambda is a compute service which run a code in response of events occurs in your resources and automatically manage that resource for you.
* Responses of lambda including capacity provisioning, OS and servers maintenance, security patch , logging and ...
* Supported programming language for lambda is JavaScript.
* Lambda designed to be available in 99.99%
* First one million request of Lambda are free and there after is 0.20 per 1 mil request

