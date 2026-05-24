# Starter Journey — Section Inventory

Source: https://github.com/databricks-solutions/starter-journey
Cloned to: `/tmp/starter-journey`
Docs path: `docs/starter-journey/docs/`

## Top-level sections (in narrative order)

| # | Section folder | What it covers |
|---|---|---|
| 1 | `before-you-start` | Pre-flight: org context, who needs to be in the room, what to gather |
| 2 | `infra-setup` | Workspaces, metastores, networking foundations |
| 3 | `data-governance-strategy` | Catalog/schema/group layout (small vs medium-large orgs) |
| 4 | `data-access-control` | Grants, groups, service principals, least-privilege |
| 5 | `access-your-data` | Ingestion paths — cloud storage, federation, streaming sources |
| 6 | `build-first-pipeline` | First end-to-end pipeline (bronze→silver→gold) |
| 7 | `orchestration` | Jobs, workflows, schedules, retries |
| 8 | `business-semantics` | Metric views, certified definitions, semantic layer |
| 9 | `databricks-aibi` | Dashboards, AI/BI, Genie spaces |
| 10 | `mlops` | Model lifecycle, MLflow, serving, evaluation |
| 11 | `cost-monitoring` | System tables, DBU tracking, alerting |
| 12 | `ci-cd-devops` | Bundles, deployment, environments, repos |

`tutorial-extras/` is the Docusaurus template scaffold — ignore.

## Narrative style observation

Each page follows the same structure:
- `**You'll understand**` one-line outcome promise + time-to-read.
- `**Prereqs**` — links to earlier sections.
- `## Why this matters` — opens with the problem, not the feature.
- `## Mental model` — the smallest mental model that holds the section together.
- `## How it works` — concrete tables, examples, naming conventions.

The Coach should mirror this style when explaining: **problem first, mental model second, mechanics last**.
