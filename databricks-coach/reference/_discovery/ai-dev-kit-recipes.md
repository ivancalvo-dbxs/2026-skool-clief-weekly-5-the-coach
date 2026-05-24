# ai-dev-kit — Skill Inventory

Source: https://github.com/databricks-solutions/ai-dev-kit
Cloned to: `/tmp/ai-dev-kit`
Skills path: `databricks-skills/`

Each skill has a `SKILL.md` with a `name`, `description`, "When to use this skill", and reference markdown files (numbered topics).

## Skills (30 total)

| Skill | Domain | What it lets Claude build |
|---|---|---|
| databricks-agent-bricks | AI/ML | Agents on Agent Bricks |
| databricks-ai-functions | AI/ML | `ai_query` and SQL AI functions |
| databricks-aibi-dashboards | Analytics/BI | AI/BI dashboards |
| databricks-apps-python | Apps | Python-based Databricks Apps |
| databricks-bundles | CI/CD | Databricks Asset Bundles for deployment |
| databricks-config | Platform | Workspace and admin config |
| databricks-dbsql | Warehousing | Databricks SQL, warehouses |
| databricks-docs | Meta | Fetch / search Databricks docs |
| databricks-execution-compute | Platform | Clusters, serverless, compute selection |
| databricks-genie | Analytics/BI | Genie spaces, NL2SQL |
| databricks-iceberg | Data engineering | Iceberg-format tables and interop |
| databricks-jobs | Orchestration | Jobs, multi-task workflows |
| databricks-lakebase-autoscale | OLTP | Lakebase autoscale |
| databricks-lakebase-provisioned | OLTP | Lakebase provisioned |
| databricks-metric-views | Semantic layer | Metric views, certified definitions |
| databricks-mlflow-evaluation | AI/ML | MLflow evaluation, scoring |
| databricks-model-serving | AI/ML | Model serving endpoints |
| databricks-python-sdk | Platform | Databricks Python SDK usage |
| databricks-spark-declarative-pipelines | Data engineering | DLT / declarative pipelines |
| databricks-spark-structured-streaming | Data engineering | Streaming with Spark |
| databricks-synthetic-data-gen | Data | Synthetic data |
| databricks-unity-catalog | Governance | UC system tables, volumes |
| databricks-unstructured-pdf-generation | Apps | PDF generation |
| databricks-vector-search | AI/ML | Vector Search |
| databricks-zerobus-ingest | Data engineering | Zerobus / low-code ingest |
| spark-python-data-source | Data engineering | Custom Spark data sources |

## How the Coach uses this

- "How do I build / implement X?" → point at the matching ai-dev-kit skill.
- The Coach does not paste code. It points to the skill and (when relevant) suggests the user install the kit so Claude can run it.
- Install reference: https://github.com/databricks-solutions/ai-dev-kit#install-in-existing-project
