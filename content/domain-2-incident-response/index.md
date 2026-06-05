---
title: "Domain 2: Incident Response"
description: Notes on cloud incident response, alert quality, investigation, containment, and recovery.
tags:
  - detection
  - incident-response
  - cloud-security
---

Incident response in AWS depends on prepared evidence, clear ownership, and reversible containment paths. The detection should tell responders what happened, where it happened, why it matters, and what the next action is.

## Response Themes

- Alert triage and severity models
- Evidence preservation before destructive containment
- IAM credential revocation and session invalidation
- Network containment for EC2, ECS, EKS, and VPC resources
- Security Hub, GuardDuty, Detective, CloudTrail, and Security Lake investigation workflows
- Post-incident control improvements as code

## Related Detection Work

- [[domain-1-detection/index|Domain 1: Detection]]
- Credential material used from new geography or ASN
- CloudTrail, Activity Log, or audit logging disabled
- New external trust relationship added to an identity provider
- Privileged role assumed by an unusual principal
- Security group or firewall rule opened broadly to the internet

## Response Notes

- Preserve relevant audit logs before rotating or deleting resources
- Revoke sessions and rotate exposed credentials early
- Capture the blast radius in terms of identities, permissions, and data access
- Write the post-incident control as code, not just as advice
