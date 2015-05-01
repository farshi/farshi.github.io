---
layout: post
title: "Amazon RDS Notes"
date: 2015-04-27 22:04:28 +1000
comments: true
categories: [Amazon RDS, AWS Architecture Certificate]
---
 Some Notes about the Amazon RDS service. You will find it useful if you want to prepare for AWS architecture certificate exam.

* Amazon RDS now supports these Relational (OLTP): MySQL, Oracle , SQL , PostgreSql, Aurora
* Supports Non-Relational No-SQL database , DynamoDB
* Supports Data warehousing database (OLAP), RedShift
* Supports ElasticCache (fast in-memory cache service), MemCached and Redis
* Aurora is a MySql Compatible Database , Not accessible yet.
* Aurora provides up to five times better performance than MySQL.
* Aurora cost is one tenth that of a commercial database while delivering similar performance and availability.
* Aurora start with 10Gb, Scales in 10 Gb increments to 64Tb.
* compute resources can scale up to 32vCPUs and 244 Gb of Memory.
* Aurora storage is self-healing. Data blocks scanned and fixed automatically.
* you can loose 2 copies without effecting write performance.
* you can loose 3 copies without effecting read performance.
* Maximum Backup Retention Period for RDS is 35 days
* When you want restore database to specific time , entirely new database will be created and endpoints will be different from the original database endpoint
* you can bring your own license in the case of selecting Oracle and SQL Server as a RDS Engine.
* You can modify MySql DB instance and increase storage size , change engine , change database name , password and ...
* Microsoft SQL server DB instance has some restrictions:
-	 You can not modify Microsoft SQL server DB instance storage size , Amazon does not support increasing storage on a SQL server DB Instance.
-	 A newly created SQL server does not contain database. you should connect from SQL server management studio and create your database.
-	The maximum storage size for SQL server DB instance is 1024 GB.
* Read replicas are available in Amazon RDS for MySQL, PostgreSQL, and Amazon Aurora. 
* Amazon RDS for MySQL and PostgreSQL allow you to add up to 5 read replicas to each DB Instance.
* With Aurora you will get 6 copies of your data (2 copies in each 3 AZ).
* Replication is only supported for the InnoDB storage engine.
* When you provision a Multi-AZ DB Instance, Amazon RDS automatically creates a primary DB Instance and synchronously replicates the data to a standby instance in a different Availability Zone (AZ)
* you can monitor you Read Replica using CloudWatch Replica Lag statistic. If a replica lags too far behind for your environment, consider deleting and recreating the Read Replica
* If you scale the source DB instance, you should also scale the Read Replicas.
*you can create ReadReplica1 from MyDBInstance, and then create ReadReplica2 from ReadReplica1. 
*Before a DB instance can serve as a source DB instance, you must enable automatic backups on the source DB instance by setting the backup retention period to a value other than 0.
