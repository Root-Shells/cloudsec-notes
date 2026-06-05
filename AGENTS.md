# AGENTS.md

This repo is a Quartz v5 site for cloud security engineering notes.

Live site:

https://sec.metoniclabs.com/

Repository:

https://github.com/Root-Shells/cloudsec-notes

## Purpose

Publish practical cloud security engineering references and posts, with a primary focus on AWS security, detection, logging, identity, infrastructure security, and enterprise security patterns.

## Repo Basics

- Content lives in `content/`.
- Quartz config lives in `quartz.config.yaml`.
- The default publishing branch is `v5`.
- GitHub Pages publishes through `.github/workflows/deploy.yml`.
- The build output is `public/`, which should not be committed.
- Installed Quartz plugins live under `.quartz/plugins/` and are controlled by `quartz.lock.json`.

## Write New Content

Create Markdown files in `content/`.

Use frontmatter like this:

```yaml
---
title: AWS Security - Example Title
description: Short page summary for previews and SEO.
tags:
  - aws
  - cloud-security
---
```

Prefer concise, useful writing:

- Start with the practical point.
- Avoid filler introductions.
- Use headings for scanability.
- Include implementation patterns, common failure modes, and checklists where useful.
- Add AWS documentation links in a `References` section for factual AWS service behavior.
- Use Mermaid diagrams when they clarify architecture, flow, troubleshooting, or ownership models.

Internal links use Quartz wiki-link syntax:

```md
[[domain-1-detection/index|Content Domain 1: Detection]]
[[domain-3-infrastructure-security/aws-ecr-security|AWS Security - ECR Secure Baseline]]
```

After adding a major page, link it from at least one topic map, usually:

- `content/index.md`
- `content/domain-1-detection/index.md`
- `content/domain-2-incident-response/index.md`
- `content/domain-3-infrastructure-security/index.md`
- `content/domain-4-identity-and-access-management/index.md`
- `content/domain-5-data-protection/index.md`
- `content/domain-6-security-foundations-and-governance/index.md`

## URL Behavior

Quartz emits non-folder notes as `.html` files, and GitHub Pages serves them without the extension.

For `content/domain-3-infrastructure-security/aws-ecr-security.md`, the public URL is:

```text
https://sec.metoniclabs.com/domain-3-infrastructure-security/aws-ecr-security
```

Do not add a trailing slash for non-folder notes. For example, this may 404:

```text
https://sec.metoniclabs.com/domain-3-infrastructure-security/aws-ecr-security/
```

## Local Commands

Install dependencies:

```bash
npm install
```

Preview locally:

```bash
npx quartz build --serve
```

Build:

```bash
npx quartz build
```

Full repo check:

```bash
npm run check
```

Format Markdown or config when needed:

```bash
npx prettier content/path-to-file.md --write
```

## Publish Workflow

Before pushing:

```bash
npm run check
npx quartz build
git status --short --branch
```

Commit and push to `v5`:

```bash
git add <files>
git commit -m "Describe the content change"
git push
```

The GitHub Actions workflow deploys automatically after push.

Verify the latest deploy:

```bash
gh run list --repo Root-Shells/cloudsec-notes --workflow deploy.yml --limit 3
gh run watch <run-id> --repo Root-Shells/cloudsec-notes --exit-status
```

Verify a published page:

```bash
curl -L --head https://sec.metoniclabs.com/<slug>
```

If local DNS routes `sec.metoniclabs.com` to homelab infrastructure, force public GitHub Pages resolution while testing:

```bash
curl --resolve sec.metoniclabs.com:443:185.199.108.153 \
  -L --head https://sec.metoniclabs.com/<slug>
```

## Design And Layout Notes

- The site intentionally disables the Quartz `recent-notes` sidebar component because it made the left sidebar too loud and pushed Explorer down.
- Keep the left sidebar simple: title, search/tools, Explorer.
- Explorer primary sections are folder notes under `content/domain-*`.
- Keep these six primary domain folders at the top level:
  - `domain-1-detection`
  - `domain-2-incident-response`
  - `domain-3-infrastructure-security`
  - `domain-4-identity-and-access-management`
  - `domain-5-data-protection`
  - `domain-6-security-foundations-and-governance`
- Prefer durable reference pages over tiny isolated notes when covering certification domains or enterprise patterns.
- Keep headings direct and technical.

## Git Safety

- Do not rewrite history unless the user explicitly asks.
- Do not revert unrelated user changes.
- Keep commits focused.
- The upstream Quartz remote may exist as `upstream`; do not push content changes there.

## Good First Checks For Agents

```bash
git status --short --branch
git remote -v
find content -maxdepth 2 -type f | sort
```

Then read the relevant topic page before editing.
