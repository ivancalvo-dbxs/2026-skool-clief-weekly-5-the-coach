# Compute Types — Essentials

**Canonical:** https://docs.databricks.com/aws/en/compute/

## The four compute flavors

| Type | Use for | Pricing shape |
|---|---|---|
| **All-Purpose clusters** | Interactive notebook work, exploration | Higher per DBU, idle costs add up |
| **Job clusters** | Scheduled jobs | Lower per DBU than All-Purpose, tear down after job |
| **Serverless (jobs, SQL, notebooks)** | Variable / bursty workloads | Per-DBU premium, but no idle cost |
| **SQL warehouses (Pro/Classic)** | BI / dashboards / SQL workloads | Sized for query throughput |

## Coach-relevant push-backs

- *"I run my scheduled job on an All-Purpose cluster."* → switch to Job cluster or serverless. All-Purpose is for humans, not jobs.
- *"Auto-terminate is set to 6 hours."* → that's a developer-comfort setting masquerading as a config. For interactive: 30 min. For jobs: doesn't apply.
- *"Serverless is always cheaper."* → no. For steady-state workloads, classic is cheaper per DBU. Serverless wins on bursty patterns.

## Decision shortcut

| Shape | Best fit |
|---|---|
| Ad-hoc interactive (analysts) | Serverless SQL warehouse or serverless notebook |
| Long batch nightly job | Job cluster (classic) |
| Short, frequent jobs | Serverless jobs |
| BI dashboards | Serverless SQL warehouse |
| ML training | Job cluster with GPU runtime |

## How the Coach uses this

Don't recommend a compute type until you know: (1) workload shape (bursty vs steady), (2) who uses it (humans vs schedules), (3) cost sensitivity.
