# CLAUDE_PROPOSAL — Building the Databricks Coach

## Goal

Build a drop-in folder for a Claude project that turns Claude into a **Databricks Coach** — one that asks before answering, routes deep questions to **Claude Specialists**, and never sounds like docs.databricks.com.

## Terminology

> **"Coach Assistants" = "Claude Specialists"**. The two terms are used interchangeably throughout this proposal and across the folder. They both refer to the same thing: topic-scoped Claude personas (one per Starter Journey section) that the Coach routes to. The Coach is the single entry point; the Claude Specialists are the domain experts the Coach delegates to internally.

## Delivery Targets (where the folder must work)

The folder must be usable in **two** environments:

1. **Claude projects (web/desktop)** — drag-and-drop the folder, every `.md` file becomes context automatically. Default target per `PROBLEM_DEFINITION.md`.
2. **Claude Code CLI** — only `CLAUDE.md` (and the files it explicitly `@imports`) auto-loads when the user `cd`s into the folder and runs `claude`. Other `.md` files are dormant unless referenced.

This means the folder needs a **`CLAUDE.md` bootstrap file** at its root that:
- Imports `identity.md`, `rules.md`, and `examples.md` via `@filename` syntax (so they auto-load in CLI).
- Tells Claude *not* to auto-read `reference/` files — those should be `Read` on demand via routing rules in `router.md`, otherwise the context window fills with material the user didn't ask about.

Without this, dragging the folder into the CLI does nothing — the user would have to manually paste files into the conversation.

## Final Deliverable

```
databricks-coach/
├── CLAUDE.md              # CLI bootstrap — @imports the files below
├── README.md              # How to use it (both Claude projects + CLI)
├── identity.md            # Who the coach is
├── rules.md               # How they coach (NOT a knowledge base)
├── examples.md            # What good looks like (coach vs. lecture)
└── reference/
    ├── router.md          # Routing rules: when to call which Claude Specialist
    ├── specialists/       # One file per Claude Specialist (= Coach Assistant)
    │   ├── data-governance.md
    │   ├── data-engineering.md
    │   ├── ai-ml.md
    │   ├── ...
    │   └── (one per section discovered in Phase 1)
    ├── sources/           # Distilled source material per topic
    │   ├── databricks-docs/
    │   ├── starter-journey/
    │   └── ai-dev-kit/
    └── frameworks/        # Reusable coaching frameworks (GROW, 5-Whys, etc.)
```

---

## Execution Plan

Each item below is an **action to run on Claude**. Items are grouped into phases. Phases run sequentially; items inside a phase can often run in parallel.

### Phase 1 — Discovery (gather raw material)

> Goal: know exactly what Claude Specialists exist and what each one is responsible for *before* writing any coach files.

- [ ] **1.1 Map the Starter Journey sections**
  - Action: `git clone https://github.com/databricks-solutions/starter-journey /tmp/starter-journey`
  - Action: List top-level sections under `docs/` (Docusaurus structure). Each top-level section = one Claude Specialist (= one Coach Assistant).
  - Output: write a list to `reference/_discovery/starter-journey-sections.md`.

- [ ] **1.2 Inventory docs.databricks.com structure**
  - Action: WebFetch `https://docs.databricks.com/aws/en/#explore-databricks` and extract the 8 "Explore Databricks" subsections.
  - Action: WebFetch `https://docs.databricks.com/aws/en/getting-started/connect/` and extract the "Develop" subsections.
  - Output: write a topic-to-URL map to `reference/_discovery/databricks-docs-map.md`.

- [ ] **1.3 Inventory ai-dev-kit recipes**
  - Action: `git clone https://github.com/databricks-solutions/ai-dev-kit /tmp/ai-dev-kit`
  - Action: List the recipes / examples available (asset types, patterns).
  - Output: write a recipe index to `reference/_discovery/ai-dev-kit-recipes.md`.

- [ ] **1.4 Cross-map**
  - Action: For each Starter Journey section (i.e., each Claude Specialist), identify (a) which docs.databricks.com pages cover the "what is", and (b) which ai-dev-kit recipes cover the "how".
  - Output: a single cross-map table at `reference/_discovery/cross-map.md`.

---

### Phase 2 — Foundation files (the coaching layer)

> Goal: the four top-level files that make Claude *behave* as a coach. Write these before touching `reference/` content — behavior first, knowledge second.

- [ ] **2.1 Write `identity.md`**
  - Action: Draft a 1-page identity covering: who the coach is, who they coach (data engineers, platform leads, architects new to Databricks), what they care about, how they speak. No Databricks marketing voice.
  - Test: Could a reader describe the coach in one sentence after reading this?

- [ ] **2.2 Write `rules.md`** *(the most important file)*
  - Action: Use the "Coach, not knowledge base" rules from `MY_IDEA.md` (section "Coach expected behaviour") as the spine.
  - Add Databricks-specific coaching rules:
    - Never paste docs. If the user asks "what is Unity Catalog?", ask why they're asking before defining it.
    - Surface tradeoffs (e.g., medallion vs. domain-driven catalogs) instead of recommending defaults.
    - When recommending an implementation path, prefer linking an ai-dev-kit recipe over writing code inline.
    - Hold accountability across turns ("last time you said you'd try X — what happened?").
  - Include the self-check test: *"Am I informing or am I coaching?"*

- [ ] **2.3 Write `examples.md`**
  - Action: Write 5–7 example exchanges. Each must show:
    - A bad (knowledge-base) response and
    - A good (coach) response to the same input.
  - Seed examples to write:
    - Catalog organization (the one already in `MY_IDEA.md`)
    - "Should I use DLT or plain notebooks?"
    - "My job is slow."
    - "I don't know if I should be on serverless."
    - "How do I do CI/CD on Databricks?"

- [ ] **2.4 Write `README.md`**
  - Action: Explain how to drop the folder into a Claude project, what to expect, and one example prompt to start with. Clarify the Coach / Claude Specialists terminology up front so users know what's going on under the hood.
  - Action: Add a "CLI usage" section: `cd databricks-coach && claude` — explains that `CLAUDE.md` bootstraps the coach automatically.

- [ ] **2.5 Write `CLAUDE.md` (CLI bootstrap)**
  - Action: Create a root `CLAUDE.md` that:
    1. Briefly states "You are the Databricks Coach. Read and apply the following files as your operating instructions."
    2. Imports the foundation files with `@identity.md`, `@rules.md`, `@examples.md` so the CLI auto-loads them.
    3. Does **not** import `reference/` files. Instead, instructs Claude to `Read` specific files from `reference/specialists/` and `reference/sources/` *only when* the routing rules in `reference/router.md` say so. Tell Claude to load `reference/router.md` on first user turn.
    4. Includes a one-line reminder: *"Before every response, ask: am I coaching or informing?"* — so the rule survives even if the imports are skimmed.
  - Test: from a fresh terminal, `cd databricks-coach && claude`, then send a Databricks question. The first reply must be a coaching question, not a definition. If it isn't, the bootstrap failed.

---

### Phase 3 — Router architecture

> Goal: the coach is the single entry point; Claude Specialists answer when called.

- [ ] **3.1 Write `reference/router.md`**
  - Action: Define explicit routing rules. For each Claude Specialist (= Starter Journey section), list trigger phrases / topics that should route to that Specialist.
  - Action: Define the handoff protocol: Coach summarizes the user's situation to the Claude Specialist, Specialist answers in coach voice (still asking questions), Coach owns the conversation thread.
  - Constraint: routing is internal — the user should never see "routing to UC Specialist". The coach voice stays consistent.

- [ ] **3.2 Define the Claude Specialist template**
  - Action: Write `reference/specialists/_TEMPLATE.md` with the structure each Claude Specialist file follows:
    - Scope (what this Specialist owns)
    - Coaching questions to ask first
    - Key tradeoffs the Specialist surfaces
    - "What is" references (links to databricks-docs/)
    - "How to" references (links to ai-dev-kit recipes)
    - Common anti-patterns / things to push back on

---

### Phase 4 — Claude Specialists (= Coach Assistants)

> Goal: one file per Claude Specialist. Each is a coach in its own right, not a doc page.

- [ ] **4.1 Generate Claude Specialist files**
  - Action: For each section identified in 1.1, instantiate the template from 3.2 into `reference/specialists/<section>.md`.
  - Action: Pull the right tradeoffs and questions from the cross-map (1.4).
  - Rule: every Claude Specialist file must include at least 3 coaching questions and at least 2 push-back patterns. No Specialist may be pure reference material.

- [ ] **4.2 Distill source material into `reference/sources/`**
  - Action: For each topic, extract just the *facts a Claude Specialist might need to ground themselves* — not full doc dumps. Aim for a one-page summary per major concept.
  - Action: Keep canonical URLs at the top of every file so the Specialist can link out when the user wants to read more.
  - Note on NotebookLM: NotebookLM has no public API. Use Claude directly to summarize fetched pages into these files instead. Cheaper, version-controllable, and avoids the dependency.

- [ ] **4.3 Add frameworks**
  - Action: Write `reference/frameworks/` files for reusable coaching moves (e.g., GROW model, 5-Whys, premortems for migrations). Keep each under one page.

---

### Phase 5 — Validation

> Goal: prove the folder produces coaching behavior, not lecturing.

- [ ] **5.1 Dry-run scenarios — both environments**
  - Action (project): Drop the folder into a fresh Claude project and run the example prompts from `examples.md`. Capture transcripts.
  - Action (CLI): From a fresh terminal, `cd databricks-coach && claude`, run the same prompts. Capture transcripts.
  - Pass criteria: in **both** environments, the Coach's first response is a question, not an answer. If CLI lectures while project coaches, the `CLAUDE.md` bootstrap is under-specified — go fix 2.5.

- [ ] **5.2 Adversarial test**
  - Action: Run prompts that *invite* lecturing: "Give me the 5 best practices for Unity Catalog." The Coach should redirect to understanding before listing.
  - Pass criteria: no transcript reads like a Wikipedia article.

- [ ] **5.3 Accountability test**
  - Action: Run a 2-turn scenario where the user commits to an action in turn 1. In turn 2, verify the Coach references the commitment.

- [ ] **5.4 Tune `rules.md`**
  - Action: Based on failures from 5.1–5.3, tighten the rules. Add specific anti-patterns observed.

---

## Open Decisions (need a call before Phase 4)

1. **Claude Specialist granularity** — one Specialist per Starter Journey top-level section, or finer? Default: top-level only, to keep the router simple.
2. **Persona** — does the Coach have a name and personality, or is it deliberately neutral? Default: neutral but direct, no name.
3. **Scope of audience** — coaching Databricks *users* (customers) or *Field Engineers*? These need different rules. Default: customers/practitioners, since that's what the Starter Journey targets.

---

## Out of Scope (this week)

- Building a hosted version with actual sub-agents / API calls. The deliverable is files, not infrastructure. The Claude Specialists are *contextual personas inside one Claude project*, not separate API endpoints.
- Covering every Databricks product surface. Coverage = Starter Journey sections only.
- Code generation. The Coach (via its Claude Specialists) points to ai-dev-kit recipes; it doesn't write SQL/PySpark inline.

---

## Success Criteria (from PROBLEM_DEFINITION.md)

> "Someone with no context should use your folder and feel **coached**, not lectured."

Final check before shipping: hand the folder to someone who has never used Databricks. If their first message gets a question back — not a paragraph of definitions — the assignment is met.
