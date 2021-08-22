---
layout: post
title:  "AWS - An Introduction to S3"
date:   2021-08-22 20:17:24 -0500
category: aws
tag: s3
---

&nbsp;
&nbsp;

***

# Summary

S3 - Simple Storage Service

An object storage service that offers:
- Industry-leading scalability, data availability, security, and performance
  - 99.9999999% durability
  - Read-after-write consistency
- Wide range of cost-effective storage classes
  - Storage lifecycle policies
  - Intelligent-tiering
  - Archive solutions
- Unmatched security, compliance, and audit capabilities
  - PCI-DSS, HIPAA/HITECH, FedRAMP, EU Data Protection Directive, and FISMA compliant
- Easily manage data and access controls
- Query-in-place and process on-request
- Most supported cloud storage service

&nbsp;
&nbsp;

***

# Provisioning a S3 Bucket

General configuration:
- Bucket name - Friendly name to give the S3 bucket
  - This must be unique across the AWS ecosystem
- AWS Region - The region in which data will be stored

Block Public Access settings for this bucket:
- Block all public access - same as enabling all the options below
  - Block public access to buckets and objects granted through new access control lists (ACLs)
    - Blocks public access to all new objects and prevent making any existing objects public in the future
    - This does not change access to any existing objects
  - Block public access to buckets and objects granted through any access control lists (ACLs)
    - Ignore all ACLs granting public access to objects
  - Block public access to buckets and objects granted through new public bucket or access point policies
  - Block public and cross-account access to buckets and objects through any public bucket or access point policies

Bucket Versioning:
  Enabling bucket versioning means multiple versions of individual objects will be stored in the same bucket. This makes it easy to preserve, retieve, and restore older versions of an object.

Default encryption:
  Enabling default encryption automatically encryptes newly stored objects. The encryption keys can be managed by S3 itself, through AWS Key Management Service, or manually.

Advanced settings:
- Object Lock - Enabling Object Lock creates a write-once-read-many (WORM) model to help prevent deletion or modification of objects for a fixed period of time. Locks can be added to individual objects in the bucket.

&nbsp;
&nbsp;

***

{% include relevant_articles.markdown title=page.title category=page.category tag=page.tag %}