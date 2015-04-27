---
layout: post
title: "Amazon VPC Notes"
date: 2015-04-27 22:08:11 +1000
comments: true
categories: [Amazon VPC , AWS Architecture Certificate]
---
Some Notes about Amazon VPC service , you will find it useful if you want to prepare for AWS architecture certificate.

Amazon VPC:

* It is like a virtual data center in cloud.
* lets you provision a logically isolated sections of the Amazon web service (AWS) cloud.
* you have complete control over your virtual network environment.
* you can select your own IP address range
* you can crate your subnets
* you can configure route tables 
* you can setup network gateways ( Internet gateway and Customer gateway).
* With VPC you have two level of security groups: 
  	-	instance security groups.
  	-	subnet network access control lists (ACLS)
* using Amazon Direct Connect you can connect your corporate data center and your AWS VPC.
* There is a default VPC which is user friendly, allowing you to immediately deploy instances.
* All Subnets in default VPC have an Internet gateway attached.
* Each EC2 instance has both a public and private IP address.
* If you delete the default CVPC the only way to get it back is to contact AWS.
* you can user VPC Peering to connect one VPC with another.
* when you use VPC Peering , instances behave as if they were on the same private network.
* you can peer even VPC's with other AWS accounts.
* Peering is in a star configuration.So Siblings VPC's can not directly talk to each other and should use centric one.
* Elastic IP addresses, Internet Gateways, VPCs per region : Max 5 
* VPC connections per region, Customer Gateways per region: Max 50
* Route tables per region: Max 200
* Security groups per VPC: Max 100
* Rules per security group: Max 50
* Dedicated VPC's are expensive because all of instance underneath will be dedicated.
* When you create a VPC, one route table and one ACL will be created automatically.
* Your subnets can not be in different availability zones , just in one AZ.
* you can have only One Internet gateway per VPC.
* putting an instance in a public subnet is not enough to make public access to that instance.you should assign public IP address to it to make it accessible from the Internet.
* For making a NAT instance as a router for routing private subnet traffic to the Internet you should disable source/destination check for that NAT instance.
* Access Control list act as firewall and rules are applied to the entire subnet.
*If you don't associate a ACL to a subnet, default ACL which allows all incoming and outgoing traffic will be assigned to a subnet automatically.
* Rules in ACL assigned order numbers, so rule #100 evaluated before rule #200. Amazon recommends to increment rules number by 100. 
* When you create a new ACL, incoming and outgoing traffic is denied by default.
* Like as a firewall , Only one ACL can be assigned to the subnet.cardinality between ACL-Subnet is Many to one.
* Amazon corporate network is segregated from AWS network.
* Port scanning is not allowed in AWS by default. for vulnerability scan you must request it from amazon for a short period.

