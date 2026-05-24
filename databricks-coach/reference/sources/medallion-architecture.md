# Medallion Architecture — Mental Model

**Canonical:** https://docs.databricks.com/aws/en/lakehouse/medallion

## The three layers

| Layer | Contents | Read by |
|---|---|---|
| **Bronze** | Raw, append-only ingest. Schema as it arrived. | Pipelines only — not analysts. |
| **Silver** | Cleaned, conformed, deduplicated. Typed. | Pipelines + advanced analysts. |
| **Gold** | Business-grade, aggregated, modeled. | Analysts, dashboards, ML. |

## Common confusions to push back on

- *"Bronze is bad data."* — No. Bronze is *unprocessed* data, with full fidelity. It's *the* source of truth for everything downstream.
- *"Bronze/silver/gold should be catalogs."* — Usually no. They should be schemas (or table-name prefixes) *inside* environment catalogs. Otherwise you lose env isolation.
- *"Gold means it's perfect."* — Gold means it's the *shape consumers expect*. It can still have bugs.

## How the Coach uses this

When a user is sketching a pipeline, ask:

- "Where does the raw data land — is that bronze for you?"
- "What's the consumer of gold — a dashboard, a model, a downstream pipeline?"
- "What does silver actually do for you that bronze doesn't?" (Sometimes the answer reveals silver is unnecessary for a simple pipeline.)

## Anti-patterns

- Skipping bronze (no raw layer to replay from).
- Letting analysts query bronze directly.
- One pipeline writing to all three layers in a single notebook (split them).
