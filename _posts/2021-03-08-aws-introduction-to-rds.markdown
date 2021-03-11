---
layout: post
title:  "AWS - An Introduction to RDS"
date:   2021-03-08 20:17:24 -0500
categories: aws rds
---
RDS - Relational Database Service

A managed Relational Database Service in AWS
- Easier to set up, operate, and scale.
- Leverage the flexibility of the cloud.
  - CPU, Memory, Storage, and IOPS can be scaled independently.
- Lower the TCO (Total Cost of Ownership)
- Offload routine database tasks such as:
  - Provisioning
  - Patching
  - Backup
  - Recovery
  - Failure Detection
  - Repair
- Enable you to focus on your core business
- Leverage the maturity of AWS operations with built in
  - High Availability (99.95% availability)
  - Cross region replication

# Provisioning an RDS Instance

Configuration Options:
- Database Engine
- License Model
- DB Engine Version
- DB Instance Class (size of server)
- Multi-AZ (Multi-availability zone) Deployment
  - Achieve high availability by having a primary instance and a synchronous secondary instance that you can fail over to when problems occur
- Storage type and size
- Backups
  - Automated backups performed when you need them or manually create backup snapshots
- Monitoring
- Maintenance Window

The above options will need to be considered based on business needs but you do not nee to worry about the underlying servers. There is no shell access available to the server. Access to some system procedures and tables that require advanced privileges are restricted.

# Terminology
Database Instance
- Basic building block of AWS RDS
- An isolated database environment
- Can contain multiple user-created databases
- Modified by using:
  - AWS CLI
  - AWS RDS API
  - AWS Management Console

Database Engine Type
- Type of Database to be run
- There are six database engines available:
  - Commercial Engines
    - Oracle
    - Microsoft SQL Server
  - Open Source Engines
    - MariaDB
    - PostgreSQL
    - MySQL
  - Cloud Native Engines
    - Aurora (MySQL and PostgreSQL compatible)

Database Instance Class
- Similar to the EC2 Instance Type
- Determines the CPU and Memory capacity

Multi AZ (Availability Zone)
- Used to create High Availability
- Means the database will be hosted in two AZs
  - An AZ is an isolated and distinct data center

Read Replicas
- A node which is part of a Database Instance
- Handles read queries
  - Assumes the most queries to a database are read queries
- Reduces burden from Primary host

Primary Host
- The node within a Database Instance which handles the traffic from a client

Secondary Host (Standby Host)
- The node within a Database Instance which is kept in sync with the primary host but does not directly handle client traffic
- Acts as a redundancy layer (takes over in case of failover)

Aurora
- MySQL and PostgreSQL compatible relational database
  - Amazon took the open source databases and swapped the storage and logging layers to be more cloud native
- Natively built for AWS