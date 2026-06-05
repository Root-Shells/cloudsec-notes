---
title: "Domain 6: Security Foundations and Governance"
description: A topic map for security foundations, governance, operating models, standards, and cloud security engineering practices.
tags:
  - cloud-security
  - security-engineering
  - governance
---

Security foundations and governance are the operating model underneath every technical control. The goal is to make secure cloud usage easier than insecure cloud usage, then verify the controls with useful signals.

## Foundation Themes

- Account vending and landing zone controls
- Control ownership and exception workflows
- Policy-as-code and secure-by-default modules
- Security service delegation and organizational guardrails
- Evidence collection for audits and operational reviews
- Metrics that show coverage, drift, and remediation quality

## Domain Map

- [[domain-1-detection/index|Domain 1: Detection]]
- [[domain-2-incident-response/index|Domain 2: Incident Response]]
- [[domain-3-infrastructure-security/index|Domain 3: Infrastructure Security]]
- [[domain-4-identity-and-access-management/index|Domain 4: Identity and Access Management]]
- [[domain-5-data-protection/index|Domain 5: Data Protection]]
- [[microservices-security/index|Microservices Security]]

## Questions Worth Writing About

- What control prevents the most damage if credentials leak?
- Which findings need an alert, and which need an automated ticket?
- Where should security live: platform modules, policy engines, CI, runtime detection, or all of the above?
- How do we keep least privilege from becoming a spreadsheet theater project?
