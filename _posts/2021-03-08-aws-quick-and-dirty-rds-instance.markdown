---
layout: post
title:  "AWS - Quick and Dirty RDS Instance"
date:   2021-03-08 20:17:24 -0500
category: aws 
tag: rds
---

&nbsp;
&nbsp;

***

# Summary 

A RDS Instance is the basic building block of RDS. It is an isolated database environment in the AWS Cloud. It can contain multiple user-created databases. 

A RDS Instance runs a database engine, determining the type of database used (PostgreSQL, MySQL, etc...). 

The computation and memory capacity are determined by the Instance class and can be selected based on requirements (the Instance class can be changed if requirements change).

A RDS Instance runs on a [VPC]({% post_url 2021-03-08-aws-quick-and-dirty-vpc %}).

&nbsp;
&nbsp;

***

# Create RDS Instance
Creates a RDS Instance.

## Prerequisites
[RDS Subnet Group]({% post_url 2021-03-08-aws-quick-and-dirty-rds-subnet-group %})

## Instructions
1. Navigate to the RDS service in the AWS Management Console.
2. Navigate to <em>Databases</em> in the left menu.
3. Click <em>Create database</em>

   ![Databases screen](/assets/aws-quick-and-dirty-rds-instance-image-databases-screen.png)

4. For <em>Choose a database creation method</em> choose <em>Easy create</em>
   - This option uses recommended best-practice configurations.
5. For <em>Engine type</em> choose the type of database to use.
6. For <em>Database instance size</em> choose an appropriate instance size.
   - The various instance sizes are helpfully named based on expected use case. i.e. Production, Dev/Test
7. <em>DB instance identifier</em> is a friendly and unique name for this instance.
8. Optionally, the <em>Master username</em> can be changed from it's default value.
9. Leave <em>Auto generate a password</em> unchecked.
10. Enter a <em>Master password</em> and <em>Confirm password</em>.
11. Click <em>Create database</em>.

&nbsp;
&nbsp;

***

# Disclaimer
This blog is a guide to quickly setup an RDS Instance in AWS and is not intended to be used for production applications. By default, data is not encrypted, deletion protection is not enabled, and the default VPC Security Group is used.

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