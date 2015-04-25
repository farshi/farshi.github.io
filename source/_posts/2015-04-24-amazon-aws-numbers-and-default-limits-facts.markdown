---
layout: post
title: "Amazon AWS Numerical Facts"
date: 2015-04-24 16:01:40 +1000
comments: true
categories: [Amazon , AWS , S3 , AWS Architecture Certificate]  
---
I've collected some information about default limits in AWS and other numerical facts from [Amazon AWS official website](http://aws.amazon.com/). May be you find it useful if you want to prepare for AWS architecture certification. This post my be updated in the future.

* AWS is available in : 190 countries
* Number of available regions: 11
* Number of Edge Locations: 52
* Availability zones per regions: two or more
* Amazon S3 designed for 11 nines durability , 99.99999999999% in a given year  
* Amazon S3 availability is 99.99% in a given year
* Objects size range in S3  is :  Min: 1 Byte , Max: 5TB
* S3 Number of buckets: 100 
* Number of objects you can store: UNLIMITED
* Glacier retrieve time : 3 to 5 Hours
* RSS type durability is %99.99 instead Standard S3 which is %99.99999999999 
* RDS size range : Min: 5GB , Max:3TB
* Aurora size range : Min: 10GB  Max: 64TB
* Aurora Availability : 2 Copies for each 3 Zone --> 6 Copies 
* Elastic Block Storage (EBS) size , Min: 1GB  , Max: 1TB
* VPC per region Max : 5 (default - increased upon request)
* Internet Gateway per region : Max 5 (default - increased upon request)
* Customer Gateways per region : Max 50 (default - increased upon request)
* Elastic IP addresses per region: Max 5 (default - increased upon request)
* Rout tables per VPC : Max 200 (default - increased upon request)
* Internet Attached to a subnet at time : Only 1
* Security group per VPC : Max 100
* Rules per Security Group: Max 50 
* VPN connections per region: Max 50
* General purpose SSD EBS availability: 99.999% 
* General purpose SSD EBS IOPS per GB: 3 to 3000 ( just for short period)
* Simple Queue Service(SQS) Message Size : Max 256 Kb 
* AWS CloudFormaton Limits : 20 stack
