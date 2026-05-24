# Unity Catalog — Essentials

**Canonical:** https://docs.databricks.com/aws/en/data-governance/unity-catalog/

## Three-tier model

`<catalog>.<schema>.<table>` — every governed object lives at this path. There is no fourth tier.

## Common layouts

| Pattern | When to use |
|---|---|
| Catalog per environment (`dev`, `staging`, `prod`) | Single team, single business unit. Starter Journey default for small orgs. |
| Catalog per BU + env (`finance_dev`, `marketing_prod`) | Multiple BUs sharing one Databricks deployment. Starter Journey "medium-large." |
| Catalog per medallion layer (`bronze`, `silver`, `gold`) | Rarely the right answer — loses environment isolation. Push back. |

## Grants

- Always grant to **groups**, not individuals.
- Always grant at **schema level or above**, not table-by-table.
- Use **service principals** for automated writes (e.g., prod pipelines).

## Volumes

Governed file storage at `/Volumes/<catalog>/<schema>/<volume>/`. Use for files that don't fit the table model (raw drops, ML artifacts, etc.).

## System tables

`system.*` schemas surface billing, lineage, audit, query history. See `system-tables.md`.

## What the Coach should never say

- "Unity Catalog is Databricks' unified governance solution for..." (doc paste — stop)
- "Here are the 5 best practices for Unity Catalog..." (lecture — stop)

## What the Coach asks

- "What are you protecting, and from whom?"
- "How many environments / BUs share this deployment?"
- "Are your grants on tables or schemas today?"
