---
layout: post
title:  "AWS - Quick and Dirty VPC Setup"
date:   2021-03-08 20:17:24 -0500
category: aws
tag: vpc
---

&nbsp;
&nbsp;

***

# Summary 

<em>Virtual Private Cloud (VPC)</em> is a service that allows the launch of AWS resources in a logically isolated virtual network you define. This gives you complete control over your virtual network environment, including selecting your own IP address range, creation of subnets, and configuration of route tables and network gateways. You can use IPv4 and IPv6 for most resources in your VPC, helping ensure secure and easy access to resources and applications.

VPCs makes it easy to customize your network configuration. Create public-facing subnets for web servers and private subnets for backend systems, such as databases or application servers. VPCs provide multiple layers of security, including security groups and <em>network access control lists (nacl)</em> to protect resources in each subnet.

The VPC Launch Wizard in the AWS Management console can be used to assist us in setting up a new VPC quickly.

&nbsp;
&nbsp;

***

# Create an Elastic IP Address
Creates an Elastic IP Address

## Instructions
1. Navigate to the VPC service in the AWS management console.
2. Navigate to <em>Elastic IPs</em> in the left menu.

   ![Elastic IPs Screen](/assets/aws-quick-and-dirty-vpc-image-elastic-ips-screen.png)

3. Click <em>Allocate Elastic IP address</em>.
4. Leave the default configuration and click <em>Allocate</em>

&nbsp;
&nbsp;

***

# Create a VPC
Creates a VPC, a public and private Subnet, a public and private Route Table, a default VPC Security Group, and a <em>Network Access Control List (NACL)</em>.

## Instructions
1. Navigate to the <em>VPC Dashboard</em> in the left menu within the VPC service.

   ![VPC Dashboard](/assets/aws-quick-and-dirty-vpc-image-vpc-dashboard.png)

2. Within the VPC Dashboard click Launch VPC Wizard.
3. Select the <em>VPC with Public and Private Subnets</em> option.
4. Click <em>Select</em>

   ![Select a VPC Configuration](/assets/aws-quick-and-dirty-vpc-image-select-vpc-configuration.png)

5. Optionally, add a unique, friendly, name for this VPC.
6. Click the input box next to <em>Elastic IP Allocation ID</em> and select the Elastic IP that was created in earlier steps.
7. Leave all other values as defaults and click <em>Create VPC</em>.

&nbsp;
&nbsp;

***

# Adding Additional Subnets
Creates an additional public and private Subnet in the VPC.

1. Navigate to <em>Subnets</em> in the left menu within the VPC service.

   ![subnets screen](/assets/aws-quick-and-dirty-vpc-image-subnets-screen.png)

2. Click <em>Create subnet<em>.
3. In the <em>VPC ID</em> drop down, select the VPC created earlier.
4. For <em>Subnet name</em> give the Subnet a friendly name identifying if this is the private or public subnet. i.e. <em>Private Subnet 2</em>.
5. For <em>Availability Zone</em> select an AZ that different from the other private and public Subnets in this VPC.
   - This information can be found by navigating to VPC Subnets screen and viewing the Subnets details.

   ![Subnet availability zone](/assets/aws-quick-and-dirty-vpc-image-subnet-details.png)

6. In <em>IPv4 CIDR block</em> choose a CIDR block that's different than the other Subnets but within the same VPC.
  - Since we kept the default values when creating the VPC
    - The VPC has a CIDR block of 10.0.0.0/16
    - The Private Subnet has a CIDR block of 10.0.1.0/24
    - The Public Subnet has a CIDR block of 10.0.0.0/24
  - In this case we can create the new Subnets with the following CIDR block.
    - The new Private Subnet should have a CIDR block of 10.0.3.0/24
    - The new Public Subnet should have a CIDR block of 10.0.2.0/24
7. Click <em>Add new subnet</em> and follow steps 4-6.
8. Click <em>Create subnet</em>.

## Updating the New Public Subnets Routing Table
The newly created Subnets are attached to the VPCs private Route Table by default. As one of the newly created Subnets is intended to public, the Routing Table, for the public Subnet, will need to be changed to the public Route Table.

### Instructions
Changing the newly created public Subnets Route Table:

1. Navigate to <em>Subnets</em> in the VPC service.
2. Find the newly created public Subnet in the list of Subnets and select it.
3. In the <em>Actions</em> drop down, in the top right, click <em>Edit route table association</em>.
4. For the <em>Route table ID</em> drop down, choose the public Route Table.
  - It's possible to figure out which Route Table is the public Route table by:
    - Navigating to <em>Route Tables</em> in the VPC service.
    - Look at the <em>Routes</em> for each Route Table associated with the VPC.
      - The public Route Table will have a Route targeting the Internet Gateway.
  - The <em>Edit route table association</em> page also shows the Routes of a given Route Table.
    - Again, the public Route Table is the one with a route targeting the Internet Gateway.

   ![route table association](/assets/aws-quick-and-dirty-vpc-image-route-table-association.png)

5. Click <em>Save</em>.
 
&nbsp;
&nbsp;

***

# Relevant Articles
<ul style="list-style-type: none;">
  {% for post in site.posts %}
    {% if post.category == page.category and post.tag == page.tag and post.title != page.title %}
      <li>
        <a href="{{ post.url }}">
          {{ post.title }}
        </a>
      </li>
    {% endif %}
  {% endfor %}
</ul>