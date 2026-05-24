# Session Memory — How the Coach Saves and Renders History

This file is loaded silently. The user never sees this file referenced. Follow every protocol below exactly.

The history lives at **`~/.databricks-coach/history.md`** on the user's local machine. No cloud storage, no auth, no network calls.

---

## Part 1 — Config check (first turn only)

On the **very first turn** of every conversation, before routing or responding to the user:

1. Check whether the history file exists:
   ```bash
   test -f ~/.databricks-coach/history.md && echo "exists" || echo "missing"
   ```

2. **If it exists:** config is ready. Proceed normally.

3. **If it does not exist:** run the setup flow below.

### Setup flow (first-time only)

Do this silently — complete it before your first coaching response.

```bash
mkdir -p ~/.databricks-coach
cat > ~/.databricks-coach/history.md << 'EOF'
# Databricks Coach History

*Sessions are logged here in descending order — newest first.*

---
EOF
```

After creating the file, tell the user in **one sentence** that their coaching history is now set up at `~/.databricks-coach/history.md` and will be saved automatically. Then proceed with the normal first-turn coaching move (ask a question back).

---

## Part 2 — Session-end detection and summarization

### When to summarize

Summarize the current topic and prepend it to the history file when **any** of these triggers occur:

| Trigger | Signal |
|---|---|
| Explicit farewell | User says: "thanks", "that's all", "goodbye", "done for now", "bye", "got it thanks", "I'm good" |
| Explicit save | User says: "save session", "save this", "end session", "log this" |
| Topic shift | User's next message routes to a **different specialist** than the one currently active |

On a **topic shift**, summarize the *just-concluded* topic first, then route the new topic to the correct specialist.

### What to include in a summary

A summary covers the conversation thread that just concluded — not the whole chat history. It is concise (5–10 bullet points total). It captures:

- The date and active specialist
- One-sentence context (what the user was actually working on)
- Key recommendations the Coach gave (what to do, and why)
- Tradeoffs that were surfaced
- Any commitments the user made ("I'll check with my admin", "I'll try serverless first")

### Summary entry format

Each entry uses this exact markdown format:

```
---
## YYYY-MM-DD | <Specialist Name> | <One-line topic title>

**Context:** <One sentence: what the user was trying to do and their situation.>

**Key recommendations:**
- <Recommendation 1>
- <Recommendation 2>
- <Recommendation 3 (add or remove bullets as needed)>

**Tradeoffs surfaced:**
- <Tradeoff 1>
- <Tradeoff 2 (omit section if none were surfaced)>

**User commitments:**
- <Commitment 1 (omit section if user made no commitments)>

---
```

### How to prepend the entry (newest first)

Use Python to insert the new entry immediately after the header block's first `---` line:

```bash
python3 - << 'PYEOF'
HISTORY_FILE = os.path.expanduser('~/.databricks-coach/history.md')

new_entry = """
---
## YYYY-MM-DD | Specialist | Topic title

**Context:** ...

**Key recommendations:**
- ...

---
"""

with open(HISTORY_FILE, 'r') as f:
    content = f.read()

marker = '\n---\n'
idx = content.find(marker)
if idx == -1:
    updated = content.rstrip() + '\n\n' + new_entry.strip() + '\n'
else:
    insert_at = idx + len(marker)
    updated = content[:insert_at] + '\n' + new_entry.strip() + '\n\n' + content[insert_at:]

with open(HISTORY_FILE, 'w') as f:
    f.write(updated)
PYEOF
```

Replace the `new_entry` string with the actual formatted entry before running.

Do this **silently**. Do not tell the user you are saving unless they ask. On an explicit farewell, you may optionally end with: *"Session saved."* (two words, no elaboration).

---

## Part 3 — Render history on demand

### Trigger phrases

The Coach renders history when the user says anything matching:

- "show history", "show my history", "show past sessions"
- "render history", "render sessions", "render recommendations"
- "visualize history", "visualize sessions", "visualize recommendations"
- "what did we cover", "past recommendations", "what have we worked on"
- "open history", "see history"

### Render protocol

1. **Read the history file:**
   ```bash
   cat ~/.databricks-coach/history.md
   ```

2. **Convert the markdown to styled HTML** and write it to `/tmp/databricks-coach-history.html`. Embed the CSS inline — do not reference any external file. Use the CSS content from `reference/templates/history-style.css`.

   Markdown-to-HTML conversion rules:
   - `# Heading` → `<h1>`
   - `## Heading` → `<h2>` (each session entry heading)
   - `**bold**` → `<strong>`
   - `- item` → `<li>` inside `<ul>`
   - `---` (horizontal rule between entries) → `<hr class="session-divider">`
   - `*italic*` → `<em>`
   - Blank lines → paragraph breaks (`<p>` or spacing)

   Each session block (content between two `---` lines) should be wrapped in `<div class="session-entry">`.

   HTML shell:
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Databricks Coach History</title>
     <style>
       /* EMBED full content of reference/templates/history-style.css here */
     </style>
   </head>
   <body>
     <div class="container">
       <!-- converted markdown content here -->
       <div class="page-footer">~/.databricks-coach/history.md</div>
     </div>
   </body>
   </html>
   ```

3. **Write the HTML file:**
   ```bash
   # (write /tmp/databricks-coach-history.html with the generated HTML content)
   ```

4. **Open in the default browser:**
   ```bash
   open /tmp/databricks-coach-history.html
   ```

5. Tell the user: *"Opening your coaching history now."* (nothing more).

---

## Part 4 — Failure handling

| Failure | What to do |
|---|---|
| History file missing at summarize time | Create it (run setup flow), then write the entry. |
| History file unreadable | Tell the user in one sentence, recreate it, and continue. |
| Write fails (permissions, disk) | Say: *"Note: couldn't save this session — check permissions on `~/.databricks-coach/`."* |
| History file empty at render time | Tell the user: *"No sessions have been saved yet."* |

Never block coaching on memory failures. Coaching always comes first.
