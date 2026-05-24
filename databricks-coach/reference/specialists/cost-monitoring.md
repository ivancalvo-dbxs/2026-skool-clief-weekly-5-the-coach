# Specialist — Cost Monitoring

## Scope

DBU tracking via system tables, cost attribution, alerting on spend, identifying expensive workloads. *Out of scope:* tuning a specific slow job (that's `orchestration` or `data-engineering`), choosing compute defaults (that's `platform-foundations`).

## Coaching questions to ask first

1. **"What surprised you about the bill, and how recent is the spike?"** — Most cost conversations start with "it went up." When did it go up? That's the diagnosis lead.
2. **"How are you attributing cost today — by workspace, by tag, by user?"** — If they have no attribution, the first work is tagging, not optimizing.
3. **"What's the budget conversation behind this?"** — Sometimes the goal is "stop the bleeding," sometimes "explain the bill to finance," sometimes "stay under X." Different responses.

## Key tradeoffs to surface

### Tradeoff 1 — Tag everything vs tag only what matters
- **Tag everything** → full attribution, but discipline burden.
- **Tag the top 20%** → catches 80% of cost, less plumbing.
- **The disambiguating question:** *"Do you need cost broken down by team / project / customer? Or just total spend trends?"*

### Tradeoff 2 — Spot/preemptible vs on-demand
- **Spot** → big savings (30–70%), interruption risk.
- **On-demand** → predictable, expensive.
- **The disambiguating question:** *"For the workloads in question — is restart-from-checkpoint acceptable, or do they need to complete in one shot?"*

## Push-back patterns

- User says: *"Cluster X is too expensive."* → Coach: *"Compared to what — what's the workload doing, and what's the latency requirement?* Cheap-but-slow might cost more in business impact than expensive-but-fast."
- User says: *"Let's just turn off all serverless."* → Coach: *"That's a hammer. What specifically is the runaway — serverless SQL, serverless jobs, or all of it? Cost system tables will tell us."*

## "What is" references (link, don't paste)

- Starter Journey — cost monitoring → /tmp/starter-journey/docs/starter-journey/docs/cost-monitoring/
- System tables overview → https://docs.databricks.com/aws/en/admin/system-tables/

## "How to" references (point at ai-dev-kit)

- System tables (billing, compute, jobs, query history) → `databricks-unity-catalog` skill (system-tables reference)

## Anti-patterns to flag

- No cluster tags → no cost attribution possible later.
- All-Purpose clusters left running overnight.
- "Auto-terminate" set to 6 hours instead of 30 minutes.
- Looking at the AWS bill instead of the Databricks system tables.

## Internal Coach notes

- The user often confuses "DBUs" with "dollars." DBUs are billing units that translate to dollars based on workload type. Don't be sloppy with this — it confuses them more.
- The system tables (`system.billing.usage`, `system.compute.*`) are the source of truth. Anything else is hearsay.
