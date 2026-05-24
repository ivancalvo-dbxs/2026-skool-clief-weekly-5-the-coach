# Cross-Map: Starter Journey → Coach Specialists → Docs → ai-dev-kit

## Decision: 8 Claude Specialists (collapsed from 12 Starter Journey sections)

The default in the proposal was "one Specialist per Starter Journey section." I collapsed adjacent sections where they form a single coaching domain. Reasoning is in the rightmost column. If you want full 1:1 fidelity, split the merged ones back out.

| # | Claude Specialist | Starter Journey section(s) | docs.databricks.com area | ai-dev-kit skill(s) | Why this grouping |
|---|---|---|---|---|---|
| 1 | **platform-foundations** | before-you-start + infra-setup | Get started, Develop | databricks-config, databricks-execution-compute | Both are pre-pipeline foundations — same coaching conversation |
| 2 | **data-governance** | data-governance-strategy + data-access-control | Data guides | databricks-unity-catalog | Layout and grants belong to the same mental model (UC) |
| 3 | **data-engineering** | access-your-data + build-first-pipeline | Data engineering | databricks-spark-declarative-pipelines, databricks-spark-structured-streaming, databricks-zerobus-ingest, databricks-iceberg | Ingestion and first pipeline are usually one decision thread |
| 4 | **orchestration** | orchestration | Data engineering | databricks-jobs, databricks-bundles | Kept separate — workflow design questions are distinct from pipeline-internal questions |
| 5 | **analytics-bi** | business-semantics + databricks-aibi | Databricks AI/BI, Data warehousing | databricks-aibi-dashboards, databricks-metric-views, databricks-genie, databricks-dbsql | Semantic layer drives BI — coached together |
| 6 | **ai-ml-ops** | mlops | AI and machine learning | databricks-mlflow-evaluation, databricks-model-serving, databricks-agent-bricks, databricks-ai-functions, databricks-vector-search | All AI/ML surface, lifecycle-flavored |
| 7 | **cost-monitoring** | cost-monitoring | Data guides (system tables) | databricks-unity-catalog (system tables) | Specific lens on system tables — narrow on purpose |
| 8 | **ci-cd-devops** | ci-cd-devops | Develop | databricks-bundles, databricks-config | Deployment and environment promotion |

## Routing heuristic (for `router.md`)

The Coach should route based on what the user is *trying to do*, not which keyword they used:

- "Where do I put my data?" → **data-governance**
- "How do I ingest from S3?" → **data-engineering**
- "My job is slow / scheduling is broken" → **orchestration**
- "Who should see this dashboard?" → **data-governance** then **analytics-bi**
- "How much is this costing?" → **cost-monitoring**
- "How do I promote dev to prod?" → **ci-cd-devops**
- "I want a chatbot over my docs" → **ai-ml-ops**

If the user is upstream of all specialists ("I just got Databricks, where do I start?") → **platform-foundations**.
