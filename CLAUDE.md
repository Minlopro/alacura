# <Client> <Project> — Instructions for Claude Code

Read this file and `README.md` at the start of every session. Before touching a feature, read `docs/features/<feature-name>/README.md`. These rules are Minlopro policy; do not relax them because a task seems small.

## Project
- Client: <Client name>, PROTO Account acronym `ACR`. Project: <Project name>.
- Technical Strategist (TS, approves all promotions and prod deploys): <name>. Functional Strategist: <name>. Client UAT owner: <name>.
- PROTO project: <link>. Technical Workbook: <Drive link>. Related repo (integration or Salesforce side): <link or "none">.
- Source-format SFDX project. Metadata lives under `force-app/`. API version in `sfdx-project.json`.

## Orgs and branches (one branch per org)
| Branch | Org | CLI alias |
|---|---|---|
| `main` | Production | `acr-prod` |
| `uat` | MLP-UAT | `acr-uat` |
| `qa` | MLP-QA | `acr-qa` |
| `activedev` | MLP-Dev | `acr-dev` (default) |

Run `sf org list` and confirm the default org is `acr-dev` before any deploy or retrieve.

## Branching rules (never break these)
1. Never commit directly to `activedev`, `qa`, `uat`, or `main`.
2. Story work happens on `feature/<story-id>-<name>` cut from `activedev` (example `feature/US-0012688-ops-request-form`). One story per branch.
3. Experiments happen on `poc/<name>` cut from `activedev`. POC code is never merged as-is; when approved, rebuild it cleanly on a new feature branch with tests.
4. Hotfixes happen on `hotfix/<story-id>-<name>` cut from `main`, never from `activedev`. After the fix reaches `main`, back-merge `main` into `uat`, `qa`, and `activedev` the same day.
5. Promotion order is fixed: `activedev` → `qa` → `uat` → `main`. Never skip. Only the TS promotes into `main`.
6. Never force-push, rebase, or rewrite history on a shared branch. Rebasing your own unpushed feature branch is fine.
7. Delete feature and POC branches after merge or abandonment.

## Pull requests
- Optional for now, strongly encouraged. Base branch is `activedev`. Use `.github/pull_request_template.md`. Link the PROTO story, describe tests and post-deploy steps.
- Never merge a PR into `qa`, `uat`, or `main` yourself. Ask the human.

## Commits
- Conventional Commits with the story ID: `feat(case): add ACR_Case_TriggerHandler (US-0012345)`, `fix(flow): ...`, `config(fields): ...`, `test(...)`, `docs(...)`.
- Small commits, one logical change each. Commit under the human's git identity; never invent an author.

## Features: two living files, always updated together
Every feature has:
- `manifest/features/<feature-name>-package.xml`: every component the feature needs to deploy, explicit names, no wildcards.
- `docs/features/<feature-name>/README.md`: what it does, scope, design, decisions (dated, with who agreed), discussions, open questions, components table, testing, deployment notes, change log.

Rules:
- New feature: copy `docs/features/_TEMPLATE/README.md` and `manifest/features/_TEMPLATE-package.xml` to the feature name (kebab-case, stable), add a row to `docs/features/README.md`. Ask the human for story IDs and a description; do not invent decisions.
- Any change to a feature's metadata updates both files **in the same commit**: add or remove the components in the manifest, update the components table and change log in the README, record any decision or open question raised in the session.
- Before working on an existing feature, read its README first and confirm your understanding with the human if anything is unclear.
- Never leave the manifest or README stale. If you are unsure whether a component belongs to a feature, ask.

## Deployments
- Sandbox deploys (`acr-dev`, `acr-qa`, `acr-uat`): only from the branch that maps to that org, only with an explicit `--manifest` (a feature manifest or `manifest/package.xml`), validate with `--dry-run` first.
- Production (`acr-prod`): humans only. Never run `sf project deploy start`, `sf project delete`, or any destructive command against `acr-prod`. If asked, stop and hand the exact command to the TS instead.
- Never deploy org-to-org. The repository is the source of truth.

## Security (hard stops)
- Never write into the repo, a commit, or a PR: passwords, tokens, SFDX auth URLs, JWT keys, consumer secrets, session IDs, client record data, CSV exports, debug logs, or hardcoded org IDs, usernames, or endpoints. Use Named Credentials, Custom Metadata, Custom Settings, or Custom Labels.
- `.sf/`, `.sfdx/`, `.env`, `*.key` stay gitignored. Do not edit `.gitignore` to include them.
- If you notice a secret already committed, stop and tell the human immediately; do not try to fix it silently.

## Code standards
- Naming: `ACR_<Object/Feature>_<Type>` for Apex (`ACR_Case_TriggerHandler`, `ACR_Case_TriggerHandlerTest`), `verbNoun()` methods, camelCase variables, `ACR_Purpose__c` custom fields, kebab-case LWC folders with an `acr` prefix.
- Apex: bulkified, one trigger per object calling a handler, no SOQL or DML in loops, CRUD/FLS and sharing enforced, no hardcoded IDs.
- Tests: 85 percent coverage on touched classes with meaningful assertions (75 percent is the Salesforce floor). Test data created in `@TestSetup`, never `SeeAllData=true`. Every new or changed LWC ships with Jest tests.
- Use the installed `sf-skills` (Apex, Flow, LWC, SOQL, objects and fields) when generating metadata rather than hand-rolling patterns.

## Definition of done for a story
Built on a feature branch, tests passing at target coverage, feature manifest and feature README updated in the same commits, no secrets, commit messages carry the story ID, branch pushed and (if used) PR opened into `activedev`. Then tell the human what changed, what to review, and any manual steps.

## When in doubt
Ask the human. Do not guess an acronym, a decision, a branch, or a target org.
