# 5 Whys — Drill to Root Cause

Use when the user presents a symptom and you suspect there's a deeper driver.

## The move

Ask "why" up to five times. Each answer becomes the input to the next question. Stop when you hit a root that's actionable.

## Example

**User:** My Databricks bill went up last month.

- **Why?** → Compute hours doubled.
- **Why?** → A new job runs nightly.
- **Why?** → Marketing wanted fresh data every morning.
- **Why?** → Their dashboard latency complaints.
- **Why?** → No one ever defined what "fresh" actually means for them.

Root: **undefined freshness SLO**. That's actionable — and very different from the surface "bill went up" framing.

## Coach reminder

- Don't actually ask "why" five times in a row. That feels like an interrogation. Vary the language: *"What's driving that?" "What's behind that?" "What changed before that started?"*
- Stop early if you hit a real constraint (legal, contractual, technical). No point drilling past a hard wall.
- The user often realizes the answer themselves around why #3 or #4. Let them.
