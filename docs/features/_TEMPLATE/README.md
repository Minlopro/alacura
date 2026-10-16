# Feature: <Feature name>

> Living document. Anyone (person or agent) should be able to read this and continue the work from where it was left. Update it in the same commit as any change to the feature, together with `manifest/features/<feature-name>-package.xml`.

| | |
|---|---|
| **Status** | Discovery / In build / In QA / In UAT / Live / On hold |
| **Feature manifest** | `manifest/features/<feature-name>-package.xml` |
| **PROTO stories** | US-00XXXXX, US-00XXXXX (links) |
| **Owner (dev)** | <name> |
| **TS / FS** | <name> / <name> |
| **Branch(es)** | `feature/US-00XXXXX-<name>` |
| **Deployed to** | Dev: yes / QA: date / UAT: date / Prod: tag vX.Y.Z |
| **Last updated** | YYYY-MM-DD by <name> |

## 1. What it does
Two or three sentences a new person can understand: the business problem, who uses it, what changes for them.

## 2. Requirements and scope
- In scope:
- Out of scope:
- Acceptance criteria (from the stories, summarized):

## 3. Solution design
How it works end to end. Objects and fields, automation (Flow / Apex / trigger), UI (LWC / page layouts / apps), permissions, integrations. Diagrams or links to the design doc go here. Name every component the same way it appears in the manifest.

## 4. Decisions
Agreed decisions with date and who agreed. Newest first.
| Date | Decision | Why | Agreed by |
|---|---|---|---|
| YYYY-MM-DD | | | |

## 5. Discussions and context
Things discussed, alternatives rejected, client preferences, constraints, gotchas found while building. Anything that would otherwise live only in someone's head or a Slack thread.

## 6. Open questions
| # | Question | Raised by | Owner | Status |
|---|---|---|---|---|
| 1 | | | | Open |

## 7. Components
Keep in sync with the manifest. List each component with its purpose so the manifest is explainable.
| Component | Type | Purpose |
|---|---|---|
| `ACR_Feature_Controller` | ApexClass | |

## 8. Testing
Test classes and what they cover, coverage on touched classes, manual test steps, smoke path, known gaps.

## 9. Deployment notes
Pre and post deploy steps (permission set assignment, custom settings, data, feature activation), dependencies on other features, destructive changes.

## 10. Change log
Newest first. One line per meaningful change, with the story or PR.
| Date | Change | Story / PR | By |
|---|---|---|---|
| YYYY-MM-DD | Created | | |
