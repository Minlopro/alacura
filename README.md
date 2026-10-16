# ACR-project-name

> **New repo checklist** (delete this block when done)
> 1. This repo was created from `Minlopro/MLP-project-template` inside the Minlopro GitHub organization, as **Private**. Name = client Account acronym from PROTO + hyphen + kebab-case project name, e.g. `PROS-service-cloud`.
> 2. Fill in this README (project block below) before anything else.
> 3. Create the branches `activedev`, `qa`, and `uat` from `main` **before committing any project code**.
> 4. Search and replace `ACR-project-name`, `ACR`, `acr-dev`, `acr-qa`, `acr-uat`, `acr-prod` across the repo with the real acronym.
> 5. Fill in the placeholders in `CLAUDE.md`.

## Project

| | |
|---|---|
| **Client** | <Client name> (PROTO Account acronym: `ACR`) |
| **Project** | <Project name> |
| **Technical Strategist** | <name> |
| **Functional Strategist** | <name> |
| **PROTO project** | <link> |
| **Project Technical Workbook** | <Drive link> |
| **Sandbox Documentation Sheet** | <Drive link> |
| **Related repositories** | <link> (if the integration team and the Salesforce team keep separate repos, each README links to the other) |

**Summary:** <two or three sentences: what is being built, for whom, target go-live>

## GitHub rules (Minlopro)

- Every repository lives in the **Minlopro GitHub organization** and is **private**. Never under a personal account or a client organization.
- Only Minlopro organization members work in a repository. One GitHub account per person, tied to your Minlopro email, 2FA on.
- Clients do not get access to Minlopro repositories by default. Code is handed over by pushing the tagged `main` to a client-owned repo at project end. Anything else needs Director approval.
- Every project repo is created from `MLP-project-template` and named `ACR-project-name`.
- Integration and Salesforce teams may keep separate repositories for the same project. Each README must link to the other.

## Branches

| Branch | Salesforce org | CLI alias | Purpose |
|---|---|---|---|
| `main` | Production | `acr-prod` | What is live. Tagged `vMAJOR.MINOR.PATCH` on every production deployment. |
| `uat` | MLP-UAT | `acr-uat` | Client acceptance testing |
| `qa` | MLP-QA | `acr-qa` | Minlopro validation |
| `activedev` | MLP-Dev | `acr-dev` | Integrated development. All feature work lands here first. |

| Short-lived | From | Into | Use |
|---|---|---|---|
| `feature/<story-id>-<name>` | `activedev` | `activedev` | All story work. One branch per story, e.g. `feature/US-0012688-ops-request-form`. Delete after merge. |
| `poc/<name>` | `activedev` | nothing | Proof of concept and spikes. Never merged as-is; approved parts move to a fresh feature branch. |
| `hotfix/<story-id>-<name>` | `main` | `main` | Urgent production fixes only. Back-merged into `uat`, `qa`, `activedev` the same day. |

**Promotion order is fixed and never skipped:** `feature/*` → `activedev` → `qa` → `uat` → `main`. A merge into a branch means a deployment to its org. Promotions into `main` are done by the Technical Strategist only, after client UAT sign-off.

## Working rules

- Nobody commits directly to `activedev`, `qa`, `uat`, or `main`.
- **Pull requests are optional for now and strongly encouraged.** When you open one: base = `activedev`, use the PR template, link the PROTO story, get one review, do not merge your own PR into `qa`, `uat`, or `main`.
- Commit small and often on your own branch, Conventional Commits with the story ID: `feat(case): add ACR_Case_TriggerHandler (US-0012345)`. Never force-push or rewrite history on a shared branch.
- Apex coverage target 85 percent on touched classes with real assertions; the Salesforce 75 percent is a floor.
- **Never commit:** passwords, tokens, SFDX auth URLs, private keys, client record data, debug logs, hardcoded org IDs or endpoints. `.sf/`, `.sfdx/`, `.env`, `*.key` are gitignored. A committed secret is an incident: tell the TS the same day, rotate, then scrub history.

## Features: one manifest and one README each

Every feature has two living files, updated together in the same commit as any change to the feature:

- `manifest/features/<feature-name>-package.xml`, e.g. `intake-form-package.xml`: every component the feature needs to deploy, named explicitly (no wildcards). This is what gets deployed when the feature is promoted.
- `docs/features/<feature-name>/README.md`: what it does, design, agreed decisions, discussions, open questions, components, testing, deployment notes, change log. Anyone (person or agent) should be able to read it and continue the work from where it was left.

To start a feature: copy `docs/features/_TEMPLATE/README.md` to `docs/features/<feature-name>/README.md` and `manifest/features/_TEMPLATE-package.xml` to `manifest/features/<feature-name>-package.xml`, then add a row to `docs/features/README.md`. Feature names are kebab-case and do not change.

Deploy one feature: `sf project deploy start --manifest manifest/features/<feature-name>-package.xml --target-org acr-qa` (validate first with `--dry-run`).

## Local setup

```bash
git clone <this repo> && cd <repo>
npm install
sf org login web --alias acr-dev --instance-url https://test.salesforce.com --set-default
sf org login web --alias acr-qa  --instance-url https://test.salesforce.com
sf org login web --alias acr-uat --instance-url https://test.salesforce.com
sf org list        # the default MUST be acr-dev
```

## Layout

```
README.md                          this file
CLAUDE.md                          instructions for Claude Code, read every session
sfdx-project.json, force-app/      source-format SFDX project
manifest/package.xml               full-project manifest
manifest/features/                 one <feature>-package.xml per feature
docs/features/<feature>/README.md  living context doc per feature
.github/pull_request_template.md
scripts/apex, scripts/soql         ad hoc scripts
```

Full policy: *Minlopro GitHub Rules* in the DoTS Technical Playbook folder. Questions: your TS, or #minlopro-technical.
