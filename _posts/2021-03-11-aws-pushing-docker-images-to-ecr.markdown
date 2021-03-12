---
layout: post
title:  "AWS - Pushing an Image to ECR"
date:   2021-03-11 20:17:24 -0500
category: aws
tag: ecr
---

&nbsp;
&nbsp;

***

# Summary 
<em>Elastic Container Registry (ECR)</em> is a fully managed container registry that makes it easy to store, manage, share, and deploy container images and artifacts anywhere.

&nbsp;
&nbsp;

***

# Terminology

Registry - a service for storing a collection of repositories.

Repository - a area to store one or more version of a specific image.

Image - an immutable snapshot of a docker virtual machine at a specific point in time.

&nbsp;
&nbsp;

***

# Prerequisites
AWS ECR\
AWS CLI\
Dockerfile

&nbsp;
&nbsp;

***

# Using an Authentication Token
Get an Authentication Token, which will have access to any ECR registry, that the IAM principal who requested the token, for 12 hours. This can be done through the AWS CLI and the resulting token can simply be piped to the **docker login** command.

Note - the username for all AWS ECRs is <em>AWS</em>

## Instructions
1. Get an authentication token from AWS ECR and use docker to login into that ECR.
  ``` sh
  aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
  ```

2. Build docker image.
  ``` sh
  docker image build -t <name>:<tag> <path_to_dockerfile>
  ```

3. Tag your image in your AWS ECR.
  ``` sh
  docker tag <aws_account_id>.dkr.ecr.<region>.amazonaws.com/<repository>:<tag>
  ```

4. Push the image to your ECR.
  ``` sh
  docker image push <repository>:<tag>
  ```

&nbsp;
&nbsp;

***

{% include relevant_articles.markdown title=page.title category=page.category tag=page.tag %}