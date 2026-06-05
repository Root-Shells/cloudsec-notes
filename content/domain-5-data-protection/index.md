---
title: "Domain 5: Data Protection"
description: Notes on protecting data in AWS with classification, encryption, access controls, monitoring, and retention patterns.
tags:
  - aws
  - data-protection
  - encryption
  - cloud-security
---

Data protection in AWS starts with knowing where sensitive data lives, who can access it, how it is encrypted, and which events prove that controls are working.

## Areas To Cover

- Data classification and discovery with Amazon Macie
- S3 Block Public Access, bucket policies, access points, and Object Ownership
- KMS key strategy, grants, key policies, and separation of duties
- Secrets Manager and Parameter Store usage patterns
- CloudTrail data events for high-risk data access
- Backup, retention, recovery, and immutability with AWS Backup and S3 Object Lock
- Data loss detection and exfiltration signals

## Review Prompts

- Which buckets, databases, and queues contain sensitive data?
- Can the data be accessed cross-account, publicly, or anonymously?
- Which principals can decrypt the data or modify the key policy?
- Are access events logged at the right level of detail?
- What is the recovery path if data is deleted, encrypted, or exposed?
