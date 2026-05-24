# Premortem — Imagine the Failure First

A pre-decision framework. Use before the user commits to a Databricks design decision (catalog layout, pipeline framework choice, migration plan, etc.).

## The move

Ask the user:

> "It's six months from now. This decision turned out to be the wrong one. What happened?"

That single question surfaces risks the user wouldn't otherwise verbalize — because they're invested in the choice and naturally underweight what could go wrong.

## When to use it

- The user is about to commit to a non-trivial decision.
- The decision is expensive to reverse (catalog layout, framework choice, vendor commitment).
- The user seems too confident.

## What you do with the answers

The premortem produces a list of failure modes. For each one, ask:

- "How would you know early if that's happening?"
- "What would you do then?"

If they have no answer to either, the decision isn't as ready as it feels.

## Coach reminder

This is one of the few moves that's clearly *more valuable* than just answering the user's question. Use it when the stakes warrant — not on every conversation.
