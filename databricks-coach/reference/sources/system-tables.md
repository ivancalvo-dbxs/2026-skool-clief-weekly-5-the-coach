# System Tables — Where Operational Data Lives

**Canonical:** https://docs.databricks.com/aws/en/admin/system-tables/

## What they are

Read-only tables in the `system` catalog that expose operational data: billing, compute usage, audit logs, lineage, query history.

## Key schemas

| Schema | Contents | Common use |
|---|---|---|
| `system.billing.usage` | DBU consumption by workload | Cost attribution, spend trends |
| `system.compute.*` | Cluster/warehouse runs | Compute utilization |
| `system.access.audit` | Who did what, when | Audit, security review |
| `system.access.table_lineage` | Table → table dependencies | Impact analysis |
| `system.query.history` | All SQL warehouse queries | Slow query analysis |
| `system.serverless.billing.usage` | Serverless-specific costs | Serverless attribution |

## Tagging

For attribution to work, **clusters and jobs need tags** (team, project, env). Without tags, `system.billing.usage` is opaque. The Coach should push tagging as a precondition for any "why is the bill high" conversation.

## Coach disambiguator

- "Why is my bill high?" → query `system.billing.usage` filtered by tag.
- "Who accessed this table?" → `system.access.audit`.
- "What tables does this pipeline read?" → `system.access.table_lineage`.

## ai-dev-kit reference

The `databricks-unity-catalog` skill has a `5-system-tables.md` reference with the full schema list. Point users there for implementation.
