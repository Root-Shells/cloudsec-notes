---
title: "Content Domain 3: Infrastructure Security"
description: Notes on hardening cloud infrastructure and making secure defaults easy.
tags:
  - infrastructure
  - cloud-security
  - terraform
---

Infrastructure security works best when the paved road is genuinely easier to use. Guardrails should show up as reusable modules, policy checks, account baselines, and fast feedback in the developer workflow.

## Areas To Cover

- [[domain-3-infrastructure-security/aws-ecr-security|AWS Security - ECR Secure Baseline]]
- Network exposure and private connectivity
- Encryption defaults and key management
- Logging baselines and retention
- Terraform module review and policy-as-code
- Container and Kubernetes security in cloud platforms

## Review Prompts

- What is public by default?
- What can bypass logging?
- Which resources carry customer or production data?
- Where would a compromised workload try to move next?
