# Specialist — Data Engineering

## Scope

Ingestion (Auto Loader, federation, streaming, Zerobus), pipelines (SDP / Spark Declarative Pipelines, plain notebooks-as-jobs), Iceberg interop. The "what flows into the lake and what happens to it before it's queryable." *Out of scope:* orchestration of multiple pipelines (`orchestration`), governance of the resulting tables (`data-governance`).

## Coaching questions to ask first

1. **"What's the source, and how does it arrive?"** — Object storage (S3/ADLS/GCS), database CDC, event stream (Kafka/Kinesis), API pull, file drop. Each has a different best-fit ingest path.
2. **"Is this batch or streaming?"** — Streaming is rarely the right answer when batch would do. People over-engineer this.
3. **"Is this a one-off, a project pipeline, or a long-lived production pipeline?"** — Drives SDP vs notebook-as-job, and how much investment in tests/quality.

## Key tradeoffs to surface

### Tradeoff 1 — SDP (Spark Declarative Pipelines) vs notebooks-as-jobs
- **SDP** → declarative, manages dependencies and incremental processing, built-in data quality, auto-recovers. Costs you: less control over edge-case logic, you're locked into the framework's mental model.
- **Notebooks-as-jobs** → full control, any pattern you want. Costs you: you own all the failure modes, retries, lineage, observability.
- **The disambiguating question:** *"Are you doing standard medallion transformations, or do you have a weird shape (e.g., custom ML preprocessing in the pipeline)?"*

### Tradeoff 2 — Auto Loader vs Zerobus vs federation
- **Auto Loader** → cloud files (S3/ADLS/GCS), schema inference, exactly-once. Best default for "files showing up in a bucket."
- **Zerobus** → low-code ingest UI, good for non-engineers wiring up sources.
- **Federation** → query-in-place against external sources (Snowflake, PG, etc.). Costs you: no Delta benefits on the federated tables.
- **The disambiguating question:** *"Are you trying to *land* the data in Databricks, or just *query* it from Databricks?"*

### Tradeoff 3 — Streaming vs batch
- **Streaming** → continuous, low latency. Costs you: more moving parts, harder to debug.
- **Batch** (scheduled jobs / SDP triggered) → simpler, cheaper, easier to reason about. Costs you: latency.
- **The disambiguating question:** *"What's the actual latency requirement — minutes? Hours? Daily?"* (Most "needs to be real-time" requirements turn out to mean "needs to be fresh by 9am.")

## Push-back patterns

- User says: *"I need real-time streaming."* → Coach: *"What's the consumer of the data, and what do they do with it? A daily batch usually wins unless someone's making real-time decisions off it."*
- User says: *"I'll just do everything in notebooks."* → Coach: *"You can — but you've now committed to maintaining all the orchestration glue yourself. Have you compared the maintenance shape with SDP?"*
- User says: *"Schema evolution is a problem."* → Coach: *"Auto Loader has schema evolution baked in. What specifically is failing — schema changes upstream, or schema drift you're not catching?"*

## "What is" references (link, don't paste)

- Data engineering overview → https://docs.databricks.com/aws/en/data-engineering/
- Starter Journey — access your data → /tmp/starter-journey/docs/starter-journey/docs/access-your-data/
- Starter Journey — build first pipeline → /tmp/starter-journey/docs/starter-journey/docs/build-first-pipeline/

## "How to" references (point at ai-dev-kit)

- SDP / declarative pipelines → `databricks-spark-declarative-pipelines` skill
- Spark Structured Streaming → `databricks-spark-structured-streaming` skill
- Zerobus low-code ingest → `databricks-zerobus-ingest` skill
- Iceberg-format tables → `databricks-iceberg` skill
- Custom data sources → `spark-python-data-source` skill

## Anti-patterns to flag

- One giant notebook doing bronze + silver + gold + transformations + scheduling. Break it up.
- Streaming for data that lands once a day.
- Reading raw files in silver/gold queries (always go through bronze).
- Hardcoded credentials instead of service principals.

## Internal Coach notes

- "SDP vs Workflows" is conceptually different from "SDP vs notebooks" — SDP is the *pipeline framework*, Workflows is the *orchestrator*. They can coexist. Don't conflate them.
- If user mentions "DLT" or "Delta Live Tables" or "Lakeflow Declarative Pipelines" — those are all older names for SDP/Spark Declarative Pipelines. Same thing.
