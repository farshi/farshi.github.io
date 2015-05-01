---
layout: post
title: "Amazon S3 Notes"
date: 2015-04-27 22:48:19 +1000
comments: true
categories: [Amazon Simple Storage Service ,S3, AWS Architecture Certificate ]
---
Some Notes about the Amazon Simple Storage Service (S3) service. You will find it useful if you want to prepare for AWS architecture certificate exam.

Amazon Simple Storage Service (S3) :

* Files can be from 1 Byte to 5 TB
* Files are stored in Buckets and there is no limit.
* Buckets name must be unique in a region level
* Buckets are private by default
* S3 Files are stored as a object in buckets
* S3 supports MultiPart uploads,It is necessary for files bigger than 5Gb
* S3 support resumable uploads.
* S3 is consistent cross availability zones
* S3 has a Life Cycle management, you can archive data after a specific time period from upload time in glacier.
* Amazon S3 designed for 11 nines durability , 99.99999999999 in a given year
* Amazon S3 availability is 99.99% in a given  year
* Files can be encrypted on S3
* S3 has versioning feature , includes all writes even if you delete an object
* When you enable the versioning , you can not disable it , but you can suspend it
* Reduced Redundancy Storage (RSS) data availability and durability is 99.99%
* RSS is suitable for storing low frequency data.
* RSS Cost: 3 Cent per GB per Month
* Glacier is a low cost storage service suitable for data archiving.
* Glacier Cost: 1 Cent per GB per Month
* Glacier retrieval time 3 to 5 Hours
