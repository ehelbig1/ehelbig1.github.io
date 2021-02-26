---
layout: post
title:  "Building a Quick and Dirty Container Service with AWS"
date:   2021-02-24 09:17:24 -0500
categories: fundamentals internet
---
# AWS Elastic Container Registry

## Creating an ECR Repository
AWS <em>Elastic Container Registry (ECR)</em> is a fully managed container registry. It allows for the storage, management, sharing, and deployment of container images and artifacts in a centralized location that can be accessed from anywhere. ECR integrates with other AWS services such as:
- Elastic Kubernetes Servicd (EKS)
- Elastic Container Service (ECS)
- Lambda
- Fargate

From the AWS console navigate to the ECR service and click on the **Create repository** button to create a new repository.

![Create repository form](/assets/create_new_ecr.png)

### General Settings
Visibility settings
- Private - Access is managed by IAM and repository policy permissions
- Public - Visible to the public and accessible for image pulls.
Repository name - A friendly name for the repository
Tag immutability (Disabled by default) - When enabled, prevents image tags from being overwritten by subsequent image pushes using the same tag.

### Image Scan Settings
Scan on push (Disabled by default) - When enabled, images are scanned after being pushed to the repository.

### Encryption Settings
KMS encryption (Disabled by default) - When enabled, you can use AWS Key Management Service to encrypt images. Uses default encryption settings otherwise.

## Pushing an Image to the Repository
Run the following commands from the command line. You will need to have the aws cli installed and configured with the correct credentials and the docker cli installed.

Authenticate your Docker cliet to your private AWS ECR registry.
```sh
aws ecr get-login-password --region <aws_region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<aws_region>.amazonaws.com
```

Build your Docker image.
```sh
docker build -t scc .
```

Re-tag your image to be in your newly created AWS ECR registry.
```sh
docker tag scc:latest <aws_account_id>.dkr.ecr.<aws_region>.amazonaws.com/scc:latest
```

Push the image to the repository.
```sh
docker push 242179911962.dkr.ecr.us-east-1.amazonaws.com/scc:latest
```


# AWS Elastic Container Service
AWS <em>Elastic Container Service (ECS)</em> is a fully managed container orchestration service. ECS provides easy integration with many other AWS services:
- Fargate
- Route 53
- Secrets Manager
- Identity and Access Management (IAM)
- CloudWatch
- Elastic Cloud Compute (EC2)
- App Mesh

ECS is also used as a building block for many of the other AWS services, making it well tested.

## Creating a Task Definition
From the AWS console, navigate to the ECS service and click on <em>Task Definitions</em> on the left navigation menu. Then click the <em>Create new Task Definition</em> button to create a new Task Definition.

![Task Definitions screen](/assets/create_a_task_definition.png)

### Select Launch Type Compatibility
Fargate - todo: explain what fargate is
EC2 - todo: explain why you would choose EC2

### Configure Task and Container Definitions
A task definition specifies which containers are included in a task and how they interact with each other.

Task Definition Name - A friendly name for the task definition.
Task Role (Optional) - An IAM role that the task will use for making API requests to authorized AWS services.
Network Mode - The Docker networking mode to use for the containers in this task. (Must be awsvpc if using Fargate)

#### Task Execution IAM Role
This role allow the task to pull container images and publish container logs to CloudWatch. A suitable role will automatically be created for you titled <em>ecsTaskExecutionRole</em> if using the AWS console.

#### Task Size
Specify a fixed size for you task. This is required if using Fargate. Container level memory settings are optional when this is set. Task size is not supported by Windows containers.

Task memory (GB) - The amount of memory allocated to the task.
Task CPU (vCPU) - The number of CPU units allocated to the task.

#### Container Definitions
A list of container definitions that are passed to the Docker daemon on a container instance.

Click <em>Add container</em>

##### Standard Configuration

Container name - A friendly name for the container.
Image - The image used to start the container. In the formation of <em>reposiotyr-url/image:tag.
Private repository authentication - When true, private repository authentication is enabled using AWS Secrets Manager.
- If selected, the ARN or name of the AWS Secrets Manager Secret will need to be provided. Todo: write information on creating the AWS Secrets Manager Secret
Memory Limits (MiB) - Define hard and/or soft memory limits in MiB for the container.
Port mappings - Port mappings allow containers to access ports on the host container for sending and recieving traffic.

##### Advanced Container Configuration
Todo!

### Service Integration
Integrate with AWS App Mesh to monitor and control microservices.

### Proxy Configuration
Configuration details for App Mesh proxy.

### Log Router Integration
FireLens for AWS ECS helps you route logs to an AWS service or AWS Partner Network (APN) destination for log storage and analysis.

### Volumes
Use a volume configuration to add volumes for use by the containers within a task.

# AWS Secrets Manager

## Creating a Secret for Private Repository Authentication
From the AWS console, navigate to the AWS Secrets Manager service and click the <em>Stroe a new secret</em> button.
Choose <em>Other type of secrets</em> for the secret type and select <em>Plaintext</em>. Enter the <em>username</em> and <em>password</em> in the text box like this:

```json
{
    "username": "privateRegistryUsername",
    "password": "privateRegistryPassword",
}
```

The username by default is <em>AWS</em> and the password can be found by the running the following command on the command line.

```sh
aws ecr get-login-password --region us-east-1
```

Click <em>Next</em> and provide a frienly name for the secret. This can optionally include a path such as: <em>production/RegistryCredentials</em>.

Any other configuration is optional. Todo: link out to article with more information on configuring AWS Secrets Manager Secrets.


A task definition is required to run Docker containers in ECS. Task definitions can define the following information:
- The Docker image to use with each container within the task.
- The amount of CPU and memory to allocate to each task or container within a task.
- The launch type, which determines the infrastructure on which the task is hosted.
- The Docker networking mode to use for the containers in the task.
- The logging configuration.
- Whether the task should continue to run if a container finishes or fails.
- The command the container should run when it is started.
- Data volumes that should be used with the containers in the task.
- The IAM role that your tasks should use.