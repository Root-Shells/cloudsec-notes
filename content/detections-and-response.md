---
title: Detections and Response
description: Notes on cloud detections, alert quality, and incident response.
tags:
  - detection
  - incident-response
  - cloud-security
---

Cloud detections should be boring in the best way: clear trigger, clear owner, clear evidence, clear next action. A detection that no one can explain at 2 a.m. is usually an expensive log search.

## Detection Ideas

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
