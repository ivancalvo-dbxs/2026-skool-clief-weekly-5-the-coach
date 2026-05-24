# Databricks Asset Bundles — Essentials

**Canonical:** https://docs.databricks.com/aws/en/dev-tools/bundles/

## What they are

YAML-defined deployment unit for Databricks assets — jobs, pipelines, dashboards, model serving endpoints, MLflow experiments. Deployed with `databricks bundle deploy`.

## Core concepts

- **`databricks.yml`** — the bundle definition (root file).
- **Targets** — environments. `dev`, `staging`, `prod` are conventional names.
- **Variables** — substituted per target (table names, cluster sizes, etc.).
- **Resources** — the assets being deployed (jobs, pipelines, etc.).

## Why the Coach recommends them

- One source of truth for what's deployed where.
- Reproducible: `databricks bundle deploy -t prod` does the same thing twice.
- Code review: the YAML is in your repo, gets reviewed like any other change.

## Common confusions

- *"Bundles vs Terraform"* — bundles handle Databricks assets natively. Terraform handles broader cloud infra. Use bundles for Databricks-native; Terraform alongside for S3, IAM, VPC.
- *"Bundles vs Git folders"* — Git folders sync notebooks for *development*. Bundles deploy artifacts for *production*. Different problems.

## Coach disambiguator

- "Should I use bundles?" → if you have more than one environment, yes.
- "Bundles or Terraform?" → both, for different things.
- "How do I parameterize prod vs dev?" → variables in the bundle, substituted per target.
