# Validation — How to Verify the Coach Works

Phase 5 of the plan. **Interactive testing must be done by you (the user).** I can write the scenarios; I cannot actually drive the Claude Code CLI or the Claude project chat from inside this build session.

Run all four checks below. If any fail, the section labeled "what to tune" tells you which file to edit.

---

## Check 1 — Dry-run (both environments)

### In Claude project (web)

1. Create a new project at https://claude.ai/projects.
2. Drag the `databricks-coach/` folder into the project's Knowledge / Files.
3. Start a chat. Paste each of these in turn (fresh chats):
   - *"I don't know if I'm organizing my catalogs the best way."*
   - *"Should I use DLT or plain notebooks?"*
   - *"My job is slow."*
   - *"How do I do CI/CD on Databricks?"*

### In Claude Code (CLI)

```bash
cd databricks-coach
claude
```

Then send the same four prompts (fresh sessions for each).

### Pass criteria

- For **all four prompts in both environments**, the Coach's **first response is a question, not an answer.**
- The Coach references the user's specific situation, not generic best practices.
- No paragraph of definitions.

### What to tune if it fails

- If the CLI lectures and the project coaches → `CLAUDE.md` bootstrap is under-specified. Add stronger framing.
- If both lecture → `rules.md` is being skimmed. Tighten Rule 1 (Ask first, advise later) and make the self-check more emphatic.

---

## Check 2 — Adversarial test

Paste these prompts. They *invite* a knowledge-base response.

- *"Give me the 5 best practices for Unity Catalog."*
- *"Explain serverless compute."*
- *"List the steps to set up MLflow."*

### Pass criteria

- The Coach refuses to list — and redirects to understanding first.
- The Coach asks why the user is asking. (Different reasons → different answers.)

### What to tune

- If the Coach gives a list → Rule 5 (Never paste docs) and Rule 1 are not landing. Strengthen the examples in `examples.md` with the exact adversarial prompt as a "✘ vs ✓" pair.

---

## Check 3 — Accountability across turns

Two-turn scenario:

**Turn 1 (you):** *"I'm setting up Databricks for my team. I'll talk to my admin tomorrow about whether I have account-level access."*

**Coach's expected behavior:** acknowledges, reflects back the commitment, and ends with something like *"OK — coming back to that next time we talk."*

**Turn 2 (you, next message):** *"OK what's next?"*

**Coach's expected behavior:** references the earlier commitment. Asks if you talked to the admin.

### Pass criteria

- The Coach names the prior commitment back to the user without being prompted.

### What to tune

- If the Coach forgets → Rule 4 (Hold accountable across turns) is not landing. Add a line in `CLAUDE.md` bootstrap explicitly reminding the model to track commitments.

---

## Check 4 — Honors "just tell me"

Prompt:

> *"Just tell me — for a 5-person team on a single workspace, what catalog layout should I use? Skip the questions."*

### Pass criteria

- The Coach **gives a direct answer** (three catalogs: dev/staging/prod). It does not interrogate the user further.
- The answer links to the Starter Journey small-orgs page, not a doc paste.

### What to tune

- If the Coach asks questions anyway → Rule 1's exception is being ignored. Strengthen the "Exception" sentence in `rules.md` Rule 1.

---

## Final smoke test

Hand the folder to someone who has **never** used Databricks. Have them send one Databricks question. Their first reply from the Coach should be a question back — not a paragraph of definitions.

If that holds, the assignment is met. From `PROBLEM_DEFINITION.md`:

> *"Someone with no context should use your folder and feel coached, not lectured."*

---

## Capturing transcripts

If you want to share results (or compare project vs CLI), save full transcripts to `databricks-coach/validation-results/` (create that dir locally — it's gitignored from the deliverable). Helpful if you need to debug coaching drift later.
