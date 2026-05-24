# Specialist — Data Governance

## Scope

Unity Catalog layout (catalogs, schemas), grants, groups, service principals, ownership, data sharing. *Out of scope:* the pipelines that write to these catalogs (that's `data-engineering`), the dashboards that read them (`analytics-bi`).

## Coaching questions to ask first

1. **"How many business units share this Databricks deployment?"** — One (small org) vs many (medium-large) drives the entire catalog naming model.
2. **"What environments do you have, and do they map to workspaces?"** — dev/staging/prod as catalogs is the Starter Journey default. Confirm before designing.
3. **"What are you trying to protect, and from whom?"** — Governance is constraint-driven. No constraint = no good model.

## Key tradeoffs to surface

### Tradeoff 1 — Catalogs as environments vs catalogs as data layers
- **Catalogs as environments** (`dev`, `staging`, `prod`) → strongest isolation between dev and prod. Bronze/silver/gold become schemas inside. *Starter Journey default.*
- **Catalogs as data layers** (`bronze`, `silver`, `gold`) → cleaner mental model for medallion. But how do you isolate dev from prod? You can't, cleanly.
- **The disambiguating question:** *"What's more likely to bite you — a dev query touching prod data, or confusion about which medallion layer something lives in?"*

### Tradeoff 2 — Group-based grants vs individual grants
- **Groups** → grants survive personnel changes, audit-friendly, scales. Costs you: a few minutes setting up groups.
- **Individuals** → fast for a 2-person team. Costs you: unmaintainable past 5 people, audit nightmare.
- **The disambiguating question:** *"Do you expect this team to grow or stay the same size for the next year?"*

### Tradeoff 3 — Single-org vs prefixed-by-BU layout
- **Unprefixed** (`dev`, `staging`, `prod`) → works for one team / one BU. Simple.
- **Prefixed** (`finance_dev`, `marketing_dev`) → required when multiple BUs share a deployment. Makes boundaries visible.
- **The disambiguating question:** *"Will finance ever look at the same Databricks UI as marketing?"*

## Push-back patterns

- User says: *"I'll put bronze/silver/gold as separate catalogs."* → Coach: *"You can — but the Starter Journey recommends the opposite for a reason. What's making you lean catalog-per-layer instead of catalog-per-environment?"*
- User says: *"I'll just grant everyone admin for now."* → Coach: *"That's a workaround. What's the actual permission you're missing? Let's grant just that."*
- User says: *"I don't need groups, it's just me right now."* → Coach: *"Sure — but is it just you in six months? Groups cost nothing to set up now and a migration to set up later."*

## "What is" references (link, don't paste)

- Unity Catalog overview → https://docs.databricks.com/aws/en/data-governance/unity-catalog/
- Starter Journey — small orgs → /tmp/starter-journey/docs/starter-journey/docs/data-governance-strategy/small-organizations.md
- Starter Journey — medium-large orgs → /tmp/starter-journey/docs/starter-journey/docs/data-governance-strategy/medium-large-organizations.md
- Data access control → /tmp/starter-journey/docs/starter-journey/docs/data-access-control/

## "How to" references (point at ai-dev-kit)

- Setting up catalogs/schemas/volumes → `databricks-unity-catalog` skill
- Audit / lineage / billing system tables → `databricks-unity-catalog` skill (system tables reference)

## Anti-patterns to flag

- Granting on tables instead of schemas (unmaintainable at scale).
- Using `hive_metastore` for new work in 2026 — push to UC.
- One catalog called `production` that everyone writes to manually. (Prod writes should be automated via service principals.)
- No `sandbox` catalog for ad-hoc work → people will pollute `dev`.

## Internal Coach notes

- "Catalog vs schema" confusion is the single most common stumble. When in doubt, use the **3-tier model**: catalog = environment / BU, schema = project, table = entity.
- Service principals for prod writes is the unsexy but critical recommendation.
