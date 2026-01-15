---
name: add-log
description: Add design/product decisions from our conversation to the decision log
accepts_args: true
---

You are helping log product and design decisions to a running decision log file.

## Context

The user is a designer/PM who discusses features and decisions with Claude. When they've reached conclusions during conversation, they use this command to document them.

## Behavior

1. **Locate the log file**

   - Search for files named `DECISIONS.md`, `decision-log.md`, or similar in the repository root or `/docs` folder
   - If multiple exist, prefer the most recently modified
   - If none exist, ask the user where to create it or use `./DECISIONS.md`

2. **What to log**

   - If the user provides explicit decisions as arguments (e.g., `/addlog search only non-archived items`), log those
   - If no arguments provided, analyze the recent conversation to extract key decisions
   - Look for conclusive statements like "we should...", "let's go with...", "the decision is..."
   - Capture both the decision and brief rationale if discussed

3. **Format**

   - Add entries with timestamp: `## DD - Month - YYYY`
   - Use bullet points for individual decisions
   - Keep entries concise - you may sacrifice grammar for brevity
   - Skip articles (a, an, the), use fragments, abbreviate when clear
   - Maintain chronological order (newest at top)

4. **Example entry**

```markdown
## 2025-01-14

- **Search**: Only non-archived items. Archived clutter results, rarely needed.
- **Archive expiration**: Non-archived expire after 10d inactivity
```

## Edge Cases

- If conversation context is unclear, ask the user to clarify what decision(s) to log
- If the log file is very large (>500 lines), suggest creating a new log for the current quarter/year
- Avoid logging exploratory discussions that didn't reach conclusions
