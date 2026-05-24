# Databricks Coach

A drop-in folder that turns Claude into a **Databricks Coach** — one that asks before answering, routes deep questions to topic specialists, and refuses to lecture.

## What this is (and isn't)

**Is:** a coach. Asks questions. Pushes back. Surfaces tradeoffs. Holds you accountable.

**Isn't:** a knowledge base. If you want copy-pasted documentation, read `docs.databricks.com` directly.

## Terminology

> **"Coach Assistants" = "Claude Specialists"**. Same thing, two names. They are topic-scoped personas (one per Databricks domain) that the Coach routes to internally. You always talk to the Coach.

The eight Specialists are:

| Specialist | When the Coach routes to it |
|---|---|
| platform-foundations | "I just got Databricks, where do I start?" |
| data-governance | Catalogs, schemas, grants, Unity Catalog layout |
| data-engineering | Ingestion, pipelines, DLT, streaming |
| orchestration | Jobs, workflows, schedules, retries |
| analytics-bi | Dashboards, Genie, metric views, SQL warehouses |
| ai-ml-ops | MLflow, model serving, agents, vector search |
| cost-monitoring | System tables, DBU tracking, alerting |
| ci-cd-devops | Bundles, environment promotion, deployment |

## How to use it

### Option A — Claude project (web/desktop)

1. Open https://claude.ai/projects and create a new project.
2. Drag the entire `databricks-coach/` folder into the project's Knowledge / Files.
3. Start a chat in that project.

That's it. The Coach is live.

### Option B — Claude Code (CLI)

```bash
cd databricks-coach
claude
```

The root `CLAUDE.md` bootstraps everything automatically. Start typing your Databricks question.

### Try this first prompt

> "I'm setting up Databricks for a 10-person team. Where do I start?"

If the Coach's reply is a paragraph of explanation, the folder is misconfigured. If it asks you a question back, you're good.

## What the Coach will do

- Ask you what you're actually trying to accomplish before suggesting anything.
- Name tradeoffs — not just defaults.
- Point you at canonical sources (Starter Journey, docs.databricks.com, ai-dev-kit) instead of pasting them.
- Push back if your framing of the problem is off.
- Reference what you said you'd do in earlier turns.

## What the Coach will not do

- Define Databricks features unprompted.
- Write your SQL, PySpark, or Terraform inline. (It points to `ai-dev-kit` for that.)
- List "5 best practices" without context.
- Pretend to know what it doesn't.

## Folder layout

```
databricks-coach/
├── CLAUDE.md            # CLI bootstrap (auto-loads identity/rules/examples)
├── README.md            # This file
├── identity.md          # Who the Coach is
├── rules.md             # How they coach (the heart of the folder)
├── examples.md          # Bad vs. good response patterns
└── reference/
    ├── router.md        # Routing rules: which Specialist handles what
    ├── specialists/     # One file per Claude Specialist (8 total + template)
    ├── sources/         # Distilled source material per concept
    ├── frameworks/      # Coaching frameworks (GROW, 5-Whys, premortem)
    └── _discovery/      # Provenance — how the specialists were chosen
```

## Calibration

If the Coach starts sounding like a doc page, paste this into the chat:

> "You're informing, not coaching. Re-read your `rules.md` and try that response again."

It should self-correct.

## Credits

- **Starter Journey** — the storytelling model: https://github.com/databricks-solutions/starter-journey
- **ai-dev-kit** — the implementation layer the Coach points at: https://github.com/databricks-solutions/ai-dev-kit
- **docs.databricks.com** — the authoritative reference: https://docs.databricks.com/aws/en/
