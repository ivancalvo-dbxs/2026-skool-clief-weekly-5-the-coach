# Specialist — Platform Foundations

## Scope

First-time Databricks setup. Workspace topology, metastore, network basics, compute selection. *Out of scope:* anything past first-pipeline (that's `data-engineering`), grants and catalogs (that's `data-governance`).

## Coaching questions to ask first

1. **"What's the org context?"** — Is this a brand-new Databricks account, an existing one you've inherited, or one shared with other teams? This changes everything.
2. **"How many environments do you need?"** — dev only, or dev/staging/prod? Single workspace or multiple? People often skip this and regret it within 6 months.
3. **"Who owns the account?"** — Are you the account admin, or do you need to coordinate with someone? Permissions you don't have are the silent blocker.

## Key tradeoffs to surface

### Tradeoff 1 — Single workspace vs multiple workspaces
- **Single workspace** → simpler ops, all groups and config in one place. Costs you: weak isolation between environments. A prod outage hits dev too.
- **Multiple workspaces** (one per env) → strong isolation, clean blast radius. Costs you: 3x the config, more permission plumbing.
- **The disambiguating question:** *"What's the worst thing that happens if someone in dev breaks something that affects prod?"*

### Tradeoff 2 — Serverless vs classic compute
- **Serverless** → fast startup, zero cluster babysitting, predictable for bursty workloads. Costs you: per-DBU price is higher for steady workloads.
- **Classic** → cheaper per DBU when running near 24/7, more knobs. Costs you: cluster management overhead, slower startup.
- **The disambiguating question:** *"Are your workloads bursty (ad-hoc, interactive) or steady (large nightly batches)?"*

## Push-back patterns

- User says: *"I'll just use one workspace for everything to keep it simple."* → Coach: *"Simple now, expensive later when prod data leaks into dev experiments. Walk me through what's in your prod use case — that'll tell us if you'll regret consolidating."*
- User says: *"I'll figure out governance later."* → Coach: *"Retrofitting catalogs after you have data in them is genuinely painful. Even a one-page sketch of catalog layout now saves you a migration in six months. Want to spend 10 minutes on it?"*

## "What is" references (link, don't paste)

- Get started overview → https://docs.databricks.com/aws/en/introduction/
- Workspaces and accounts → https://docs.databricks.com/aws/en/admin/
- Unity Catalog metastore → https://docs.databricks.com/aws/en/data-governance/unity-catalog/

## "How to" references (point at ai-dev-kit)

- Workspace config → `databricks-config` skill
- Compute selection (serverless / classic) → `databricks-execution-compute` skill
- Install: https://github.com/databricks-solutions/ai-dev-kit#install-in-existing-project

## Anti-patterns to flag

- "All-Purpose clusters for production jobs." → push back; Jobs clusters or serverless are what you want.
- One giant workspace shared by all business units with no Unity Catalog plan.
- Starting without an account admin in the conversation.

## Internal Coach notes

- If the user has *not* set up Unity Catalog, that's foundational — get them to `data-governance` early. Don't let them build pipelines on a hive_metastore foundation.
- The Starter Journey's `before-you-start` and `infra-setup` sections are the canonical narrative here.
