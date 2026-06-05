---
title: AWS Security - Centralize Root Access in Organizations
description: Enable centralized root access and delegate IAM administration so member account root credentials can be removed and privileged actions can be tightly controlled.
tags:
  - aws
  - iam
  - organizations
  - cloud-security
---

In a multi-account AWS organization, every member account has a root user. Leaving those root credentials recoverable account-by-account is unnecessary risk. A better baseline is to enable centralized root access, delegate IAM administration to a security account, and remove root credentials from member accounts.

The goal is simple: member account root access should be exceptional, temporary, scoped, and logged.

## Recommended Pattern

Use the AWS Organizations management account only to enable the feature and register a delegated administrator. Put ongoing root access management in a dedicated security or identity account.

Enable both IAM root access capabilities:

- Root credentials management: lets the management account or IAM delegated administrator remove root credentials from member accounts.
- Privileged root actions in member accounts: lets authorized operators perform specific root-only tasks through short-term privileged sessions.

Then register a delegated administrator for IAM. This keeps daily security operations out of the Organizations management account.

## Why This Matters

Centralized root access changes the root-user model for member accounts:

- Existing member account root passwords, access keys, signing certificates, and MFA devices can be removed.
- New AWS Organizations member accounts are created without root credentials by default.
- Member accounts cannot sign in as root or recover root credentials unless an authorized privileged action allows recovery.
- Root-only tasks can be performed through short-term, task-scoped sessions instead of permanent root sign-in.

That is a better failure mode. An attacker who compromises a member account email inbox or finds an old root access key has less to work with.

## CLI Setup

Run the enablement from the Organizations management account with an IAM role that has the required IAM and Organizations permissions.

```bash
aws organizations enable-aws-service-access \
  --service-principal iam.amazonaws.com
```

Enable centralized management of member account root credentials:

```bash
aws iam enable-organizations-root-credentials-management
```

Enable privileged root sessions for supported root-only tasks:

```bash
aws iam enable-organizations-root-sessions
```

Register the IAM delegated administrator. Replace the account ID with your security or identity account:

```bash
aws organizations register-delegated-administrator \
  --service-principal iam.amazonaws.com \
  --account-id 111111111111
```

After this, use the delegated administrator account for root credential audits and privileged tasks.

## What To Do Next

Inventory root credential state across member accounts. AWS provides `IAMAuditRootUserCredentials` for auditing root credential status during a privileged task, and `iam:GetAccountSummary` is enough for the consolidated credential summary.

Remove root credentials from member accounts where possible. For privileged sessions that delete member account root credentials, scope the session with the AWS managed root task policy:

```bash
aws sts assume-root \
  --target-principal 111122223333 \
  --task-policy-arn arn=arn:aws:iam::aws:policy/root-task/IAMDeleteRootUserCredentials \
  --duration-seconds 900
```

Use a Regional STS endpoint for `AssumeRoot`; the global STS endpoint is not supported for this operation.

## Guardrails

- Do not use the Organizations management account for routine administration.
- Grant `sts:AssumeRoot` only to tightly controlled roles in the delegated administrator account.
- Use `sts:TaskPolicyArn` conditions where possible to limit which privileged root task policies can be used.
- Alert on `AssumeRoot` in CloudTrail.
- Keep root credential recovery as an exception process, not a standing access path.
- Delete member account root credentials again after any task that requires recovery.

## Caveats

Centralized root access does not eliminate every root-only operation. Some tasks still require signing in as the account root user. When that happens, allow password recovery for the member account, complete the task, then remove root credentials again.

This control is not a replacement for protecting the Organizations management account. The management account still needs strong MFA, restricted access, monitored break-glass paths, and minimal day-to-day use.

## References

- [Centralize root access for member accounts](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-enable-root-access.html)
- [Perform a privileged task on an AWS Organizations member account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user-privileged-task.html)
- [AWS IAM and AWS Organizations](https://docs.aws.amazon.com/organizations/latest/userguide/services-that-can-integrate-iam.html)
- [AWS STS AssumeRoot API](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoot.html)
