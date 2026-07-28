---
title: AWS Core Services
description: Extremely high-level one-line reference for common AWS services across compute, networking, databases, storage, security, monitoring, automation, and events.
tags:
  - aws
  - cloud
  - fundamentals
---

AWS has a lot of services, but most cloud architecture conversations start with a small core set.

## Compute

| Service | One-Line Summary                                                                   |
| ------- | ---------------------------------------------------------------------------------- |
| EC2     | Virtual machines that give you configurable compute capacity in AWS.               |
| Lambda  | Serverless functions that run code in response to events without managing servers. |
| ECS     | AWS-managed container orchestration for running Docker containers.                 |
| Fargate | Serverless compute for containers, commonly used with ECS or EKS.                  |
| EKS     | Managed Kubernetes for running containerized workloads on AWS.                     |

## Networking

| Service     | One-Line Summary                                                             |
| ----------- | ---------------------------------------------------------------------------- |
| VPC         | Isolated virtual network where AWS resources are placed and connected.       |
| Route 53    | Managed DNS and domain routing service.                                      |
| CloudFront  | Content delivery network that caches and serves content from edge locations. |
| ELB / ALB   | Load balancing services that distribute traffic across targets.              |
| API Gateway | Managed front door for creating, publishing, securing, and monitoring APIs.  |

## Databases

| Service     | One-Line Summary                                                                         |
| ----------- | ---------------------------------------------------------------------------------------- |
| RDS         | Managed relational databases such as PostgreSQL, MySQL, MariaDB, Oracle, and SQL Server. |
| DynamoDB    | Serverless NoSQL key-value and document database.                                        |
| ElastiCache | Managed in-memory caching with Redis OSS, Valkey, or Memcached-compatible engines.       |

## Storage

| Service    | One-Line Summary                                                        |
| ---------- | ----------------------------------------------------------------------- |
| S3         | Object storage for files, logs, backups, static assets, and data lakes. |
| EBS        | Block storage volumes attached to EC2 instances.                        |
| S3 Glacier | Low-cost archival storage classes for long-term retention.              |

## Security and Identity

| Service         | One-Line Summary                                                              |
| --------------- | ----------------------------------------------------------------------------- |
| IAM             | Identity and access management for AWS users, roles, groups, and permissions. |
| Cognito         | Managed user sign-up, sign-in, and identity federation for applications.      |
| KMS             | Key management service for creating and controlling encryption keys.          |
| Secrets Manager | Managed storage, rotation, and retrieval of application secrets.              |
| WAF             | Web application firewall for filtering HTTP and HTTPS requests.               |

## Management and Monitoring

| Service        | One-Line Summary                                                              |
| -------------- | ----------------------------------------------------------------------------- |
| CloudWatch     | Metrics, logs, alarms, dashboards, and operational monitoring.                |
| CloudTrail     | Audit log of AWS API activity across accounts and services.                   |
| Config         | Resource inventory, configuration history, and compliance evaluation.         |
| CloudFormation | Infrastructure-as-code service for provisioning AWS resources from templates. |
| CodePipeline   | Managed continuous delivery pipeline service.                                 |

## Events and Workflows

| Service        | One-Line Summary                                                                     |
| -------------- | ------------------------------------------------------------------------------------ |
| SQS            | Managed message queue for decoupling producers and consumers.                        |
| SNS            | Pub/sub messaging service for fanout notifications.                                  |
| EventBridge    | Event bus for routing events between AWS services, applications, and SaaS providers. |
| Step Functions | Serverless workflow orchestration for coordinating distributed application steps.    |

## Quick Mental Model

- Compute runs workloads.
- Networking connects and exposes workloads.
- Databases store structured application state.
- Storage holds objects, files, archives, and block volumes.
- Security and identity decide who can do what and how secrets and keys are protected.
- Management and monitoring show what exists, what changed, and what happened.
- Events and workflows decouple systems and coordinate multi-step processes.
