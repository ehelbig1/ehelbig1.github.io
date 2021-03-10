---
layout: post
title:  "AWS - Quick and Dirty RDS Subnet Group"
date:   2021-03-08 20:17:24 -0500
categories: aws cloud
---
A RDS Database Subnet Group is a collection of subnets (typically private) that are designated for a RDS Instance. RDS Subnet Groups specify a particular VPC a RDS Instance is created in.

Each RDS Subnet Group should have subnets in at least two Availability Zones (AZ). Amazon chooses a subnet and an IP address within that subnet from the RDS Subnet Group  to associate with the RDS Instance. If the primary database of a Multi-AZ deployment fails, Amazon can promote the secondary database to primary and create a new secondary database in another AZ, specified by the RDS Subnet Group.

When Amazon RDS creates a RDS Instance, it assigns a network interface to the Instance by using an IP address from the RDS Subnet Group. It is recommended to use the DNS name to connect to the RDS Instance as the underlying IP address can change.

# Create RDS Subnet Groups
Creates a RDS Subnet Group

## Prerequisites
[VPC]({% post_url 2021-03-08-aws-quick-and-dirty-vpc %})

## Instructions
1. Navigate to the RDS service in the AWS Management Console.
2. Navigate to <em>Subnet groups</em> in the left menu.

![Subnet groups screen](/assets/aws-introductions-to-rds-image-subnet-groups-screen.png)

3. Click <em>Create DB Subnet Group</em>.
4. Give the subnet group a friendly name and description.
5. Select the VPC ID of the VPC created in earlier steps.
6. In the drop down for <em>Availability Zones</em> select each availability zone.
   - The VPC Subnets that were created earlier are located in specific Availability Zones (AZ).
     - This information can be found by viewing the Subnets in the VPC service.
   - By selecting all AZs, these Subnets will appear regardless of which AZ they are in.
7. In the drop down for <em>Subnets</em> select the two private Subnets created in earlier steps.
   - Unfortunately, the names of Subnets are not shown in this drop down.
   - Navigate to the Subnets in the VPC service to determine the Subnet ID of the Private Subnet.
   - You can duplicate the current tab to navigate to other services without losing information.
8. Click <em>Create</em>