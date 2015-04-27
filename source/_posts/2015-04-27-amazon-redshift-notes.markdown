---
layout: post
title: "Amazon RedShift Notes"
date: 2015-04-27 22:06:33 +1000
comments: true
categories: [Amazon RedShift, AWS Architecture Certificate]  
---
Some Notes about Amazon RedShift service , you will find it useful if you want to prepare for AWS architecture certificate.

Amazon RedShift is the Amazon warehouse service in cloud, characteristics are :

* fast and powerful
* fully managed service
* Peta byte scale 
* Instead of storing data as a series of rows, it has columnar data storage
* Column-based storage require far fewer I/Os so greatly improving query performance 
* 10 Time Faster than traditional data warehouses.
* in column-base storage , similar data is stored sequentially , so data can be compressed much more than row-base storage,
* RedShift automatically samples your data and select the most appropriate compression schema.
* Redshift has MPP ( Massively Parallel Processing) feature.
* by MPP , data are distributed and queries load across all nodes to your data
* cost is less than a tenth of most other data warehouse solutions.
* configuration of RedShif can be Single node(160Gb) or Multi-node.
* in Multi-Node mode , One node is Leader node , and then you can create  up to 128 compute nodes.
* Leader node manages client connections and receives queries
* Computed Node store data and perform queries and do computations
* You will not charged for leader node hours; only compute nodes will incur charges.
* Encrypted in transit using SSL.
* Encrypted at rest using AES-256
* By default Redshift takes care of key management
* You can manage your own keys through HSM or by Amazon Key Management Service.
* Currently only available in 1 AZ
* Can restore snapshots to new AZ's in the event of outage.
* Fail over is not automatically , it's manual.