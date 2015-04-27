---
layout: post
title: "Amazon RDS Notes"
date: 2015-04-27 22:04:28 +1000
comments: true
categories: [Amazon RDS, AWS Architecture Certificate]
---
 
 Some Notes about Amazon RDS service, you will find it useful if you want to prepare for AWS architecture certificate.


 * Amazon RDS now supports these Relational (OLTP): MySQL, Oracle , SQL , PostgreSql, Aurora
* Supports Non-Relational No-SQL database , DynamoDB
* Supports Data warehousing database (OLAP), RedShift
* Supports ElasticCache (fast in-memory cache service), MemCached and Redis
* Aurora is a MySql Compatible Database , Not accessible yet.
* Aurora provides up to five times better performance than MySQL.
* Aurora cost is one tenth that of a commercial database while delivering similar performance and availability.
* Aurora start with 10Gb, Scales in 10 Gb increments to 64Tb.
* compute resources can scale up to 32vCPUs and 244 Gb of Memory.
* with Aurora you will get 6 copies of your data (2 copies in each 3 AZ).
* Aurora storage is self-healing. Data blocks scanned and fixed automatically.
* you can loose 2 copies without effecting write performance.
* you can loose 3 copies without effecting read performance.
* Maximum Backup Retention Period for RDS is 35 days
* When you want restore database to specific time , entirely new database will be created and endpoints will be different from the original database endpoint
* you can bring your own license in the case of selecting Oracle Database as a RDS Engine.
* You can modify MySql DB instance and increase storage size , change engine , change database name , password and ...
* Microsoft SQL server DB instance has some restrictions:

-	 You can not modify Microsoft SQL server DB instance storage size , Amazon does not support increasing storage on a SQL server DB Instance.
-	 A newly created SQL server does not contain database. you should connect from SQL server management studio and create your database.
-	The maximum storage size for SQL server DB instance is 1024 GB.

