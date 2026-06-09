## **asg.yaml – Infrastructure Overview**

This CloudFormation template provisions a scalable and highly available web application infrastructure on AWS.

Architecture

The stack includes:

VPC with 2 public subnets
Internet Gateway
Security Group
Application Load Balancer (ALB)
Target Group
EC2 Launch Template (Apache web servers)
Auto Scaling Group (ASG)
CloudWatch Alarm
Scaling Policy

The application runs an Apache web server on EC2 instances distributed across two availability zones. An Application Load Balancer handles incoming traffic and distributes it across healthy instances registered in a Target Group.

Auto Scaling & Monitoring

CPU utilization is monitored using a CloudWatch alarm:

Metric: CPUUtilization (average over 5 minutes)
Threshold: 70%
Condition: 3 consecutive evaluation periods above threshold

When triggered, a scaling policy increases the number of EC2 instances to handle higher load.

Scaling Limits
Minimum capacity: 1 instance
Maximum capacity: 3 instances
Purpose

This setup ensures high availability, automatic scaling based on load, and cost efficiency for a simple web application deployed inside a single VPC.


## **ec2.yaml – Infrastructure Overview**

The infrastructure described in the `ec2.yaml` file is intended to deploy a similar set of cloud resources, except for the Auto Scaling configuration.

Instead, it demonstrates load balancing of incoming traffic between two web services running in two different Availability Zones.

## **iam.yaml**

The `iam.yaml` file provisions IAM resources including:

- an IAM user with administrator access
- an IAM group for the development team, which the user is added to
- an IAM role
- an IAM policy that grants full access to an S3 bucket for the group and its users

## **rds.yaml**

The `rds.yaml` file provisions an Amazon RDS database instance using the MySQL engine.

The database is used as a backend data store for the application.

## **s3-bucket.yaml and s3-static.yaml**

These files provision S3 buckets with basic configuration.

The difference is that `s3-static.yaml` also enables static website hosting using an `index.html` file.

## **vpc.yaml**

The `vpc.yaml` file provisions the network infrastructure for a secure and highly available application. It includes:

- a VPC
- 2 public subnets
- 2 private subnets for application workloads
- 2 private subnets for databases
- an Internet Gateway
- a Bastion Host for accessing application instances
- security groups allowing SSH access from the Bastion Host to one application instance and ICMP access to the second instance in a private subnet
- 2 application EC2 instances deployed in private subnets

---

## Public Subnets Usage

The infrastructure originally included two public subnets, each intended to host a separate entry point.

During the design iteration, the architecture was simplified and consolidated to use a single Bastion Host located in `PublicSubnet1A` for secure access to private resources.

As a result, `PublicSubnet1B` is currently not used by any active resources. It remains part of the VPC design for potential future expansion and high availability scenarios (e.g., load balancer deployment or an additional Bastion Host setup).