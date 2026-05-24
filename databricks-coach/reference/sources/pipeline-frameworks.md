# Pipeline Frameworks — Essentials

## DLT / Spark Declarative Pipelines (newer name: Lakeflow Declarative Pipelines)

**Canonical:** https://docs.databricks.com/aws/en/dlt/

- Declarative — you define tables and expectations; the framework manages dependencies, retries, incremental processing.
- Built-in data quality (`@dlt.expect`).
- Best for **standard medallion transforms** (bronze→silver→gold).
- Worst for unusual patterns that don't fit table-to-table flow (heavy ML preprocessing, weird side effects).

## Notebooks-as-jobs

- A notebook or `.py` file run by a Databricks Job.
- Full control. Any pattern works.
- You own retries, dependencies, observability, schema management.
- Best when the pipeline is one-off, unusual, or already exists.

## Spark Structured Streaming

**Canonical:** https://docs.databricks.com/aws/en/structured-streaming/

- For genuine continuous ingestion.
- Often used inside DLT for streaming sources (Auto Loader, Kafka).
- Doesn't replace DLT — composes with it.

## Auto Loader

**Canonical:** https://docs.databricks.com/aws/en/ingestion/auto-loader/

- Files-from-cloud-storage ingest. Handles schema inference, schema evolution, exactly-once.
- Default answer for "files showing up in S3/ADLS/GCS."
- Composes with both DLT and notebooks.

## Coach disambiguator

- "Should I use DLT or notebooks?" → ask: *"Is this a standard medallion shape, or do you have unusual transformations?"* + *"Long-lived production or one-off?"*
- "Should I use streaming or batch?" → ask: *"What's the actual latency requirement?"* (Most don't need streaming.)
