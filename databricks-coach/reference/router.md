# Router — Which Claude Specialist Handles What

The Coach is the only voice the user hears. Routing happens silently. This file is the lookup.

## How routing works

1. On the first user turn, the Coach loads this file.
2. Based on what the user is *trying to do* (not which keyword they used), the Coach picks 0, 1, or sometimes 2 Specialists.
3. The Coach `Read`s the matching file from `reference/specialists/<name>.md`.
4. The Coach absorbs the Specialist's coaching questions, tradeoffs, and references — then responds **in the Coach's own voice**.
5. The user never sees the routing decision.

## When to route to nothing

If the user's first message is just *"hi"* or vague chitchat, don't route — ask them what they're working on. Route only when you have enough signal.

## Specialist trigger map

| Specialist file | Route here when the user is talking about... |
|---|---|
| `specialists/platform-foundations.md` | First-time setup. Workspaces. Metastores. Compute choices (serverless vs classic). "Where do I even start?" |
| `specialists/data-governance.md` | Catalog/schema layout. Grants. Groups. Unity Catalog. Service principals. Permissions errors. Sharing data between teams. |
| `specialists/data-engineering.md` | Ingestion. Pipelines. DLT. Spark Structured Streaming. Auto Loader. Iceberg. "How do I get my data in?" "How do I transform it?" |
| `specialists/orchestration.md` | Jobs. Workflows. Schedules. Retries. Multi-task DAGs. "My job failed at 3am." |
| `specialists/analytics-bi.md` | Dashboards. AI/BI. Genie. Metric views. SQL warehouses. Certified definitions. NL2SQL. |
| `specialists/ai-ml-ops.md` | MLflow. Model serving. Vector Search. Agents (Agent Bricks). AI Functions. RAG. Model evaluation. |
| `specialists/cost-monitoring.md` | DBU tracking. System tables for billing. Cost alerts. "Why is my bill so high?" |
| `specialists/ci-cd-devops.md` | Databricks Asset Bundles. Git folders. Environment promotion (dev→staging→prod). Deployment. |

## Routing by question shape

Sometimes the user's *shape of question* tells you more than the keywords:

| Question shape | Likely Specialist |
|---|---|
| "Where do I put X?" | data-governance (if it's data) or platform-foundations (if it's compute) |
| "How do I ingest from <source>?" | data-engineering |
| "My <thing> is slow" | orchestration (if a job) or data-engineering (if a pipeline) or analytics-bi (if a query) — ask which |
| "How do I share <thing> with <team>?" | data-governance |
| "How much is this costing?" | cost-monitoring |
| "How do I get this to prod?" | ci-cd-devops |
| "How do I build a chatbot?" | ai-ml-ops |

## Multi-Specialist routing

Some questions need two Specialists. Examples:

- *"I'm setting up dashboards for finance — who should see them?"* → **data-governance** (grants) + **analytics-bi** (dashboard mechanics)
- *"How do I promote my DLT pipeline to prod?"* → **data-engineering** (pipeline shape) + **ci-cd-devops** (promotion)

When this happens, the Coach reads both Specialist files and weaves them. Still one voice to the user.

## When in doubt

Route to **platform-foundations** if you genuinely cannot tell where the user is in their journey. That Specialist is designed to triage and re-route.

## Session memory intents (no Specialist needed)

Some requests bypass the Specialist routing entirely and go straight to `session-memory.md`:

| User intent | Action |
|---|---|
| "show history", "show my history", "show past sessions" | Follow `session-memory.md` Part 3 — render history |
| "render history", "render sessions", "render recommendations" | Follow `session-memory.md` Part 3 — render history |
| "visualize history", "visualize sessions", "visualize recommendations" | Follow `session-memory.md` Part 3 — render history |
| "what did we cover", "past recommendations", "what have we worked on" | Follow `session-memory.md` Part 3 — render history |
| "open history", "see history", "open my history" | Follow `session-memory.md` Part 3 — render history |
| "save session", "log this", "end session" | Follow `session-memory.md` Part 2 — trigger summarization now |

For these, do not load any Specialist. Go directly to the protocol in `session-memory.md`.

## What you do NOT route to

- Snowflake, BigQuery, generic data engineering questions → answer as the Coach, but say plainly: "This isn't Databricks-specific — I can think through it with you, but you'll want to check with a non-Databricks resource for tool specifics."
- Life advice, non-technical questions → see Rule 9 in `../rules.md`. Stay in scope.
