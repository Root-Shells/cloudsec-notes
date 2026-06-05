---
title: "Domain 4: Identity and Access Management"
description: Notes on cloud IAM, federation, privilege boundaries, and access review.
tags:
  - iam
  - cloud-security
---

Identity is the control plane. In cloud environments, most major incidents eventually become an IAM story: a trusted principal, an overbroad permission, a weak boundary, or a missing detection.

## Notes To Develop

- [[domain-4-identity-and-access-management/aws-centralized-root-access|AWS Security - Centralize Root Access in Organizations]]
- Human access patterns: SSO, break-glass, MFA, and session duration
- Workload identity: roles, service accounts, managed identities, and federation
- Permission design: least privilege, privilege boundaries, and scoped automation
- Review cadence: detecting unused access and risky grants

## Useful Checks

- Can this principal create or modify other principals?
- Can it pass roles, assume roles, or mint credentials?
- Can it disable logging, encryption, or network controls?
- Is the access temporary, attributable, and observable?
