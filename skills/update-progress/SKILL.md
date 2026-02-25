---
name: update-progress
description: Generates or updates docs/implementation-status.md with a feature-level snapshot of what's built, partial, stubbed, or missing. Use when starting a new session on a project, after completing a feature, or when a new agent needs project context. Triggers on "update progress", "update implementation status", "update impl status", "what's built", "generate implementation status", or "sync impl status".
---

# Update Progress

Generate or refresh `implementation-status.md` — a fast-load context document for new sessions and agents.

## Workflow

### Step 1: Analyze the conversation

Read the current conversation to identify what was just implemented: components, endpoints, functions, data flows, architecture patterns. If the user shared a Jira or Linear ticket or described work in their prompt, use that as your primary signal for both what was built AND what was intentionally left out of scope. Never mention ticket identifiers in the file.

### Step 2: Find the docs directory

Search for the project's docs folder:

```
Glob: docs/   OR   .docs/   OR   documentation/
```

If neither exists, create `docs/` and note it in your response.

### Step 3: Read the existing status file

Read `docs/implementation-status.md` to understand the current documented state before making any changes. If the file doesn't exist yet, you'll create it from scratch.

### Step 4: Update the file

Write to `docs/implementation-status.md` (or `.docs/implementation-status.md`).

**Target: under 150 lines. Every line must earn its place.**

When updating:
- Add new information to existing sections rather than creating duplicates
- If a component or feature already has an entry but now has more detail, enhance that entry in place
- If something was `Partial` or `Stubbed` and is now built, update its status
- Add entirely new rows only for things not previously documented

Use grep for additional signals in the codebase (secondary to what the user told you):
```
pattern: TODO|FIXME|stub|not implemented|placeholder|hardcoded
```

Use this format, adapting section names to the project type:

```markdown
# Implementation Status

_Last updated: YYYY-MM-DD_

## Infrastructure

| Area | Status | Notes |
|---|---|---|
| [Component] | [Status] | [Key types/classes/tables involved] |

## Screens & Flows

| Screen | Status | What works | What's missing |
|---|---|---|---|
| [Screen name] | [Status] | [Comma-separated working parts] | [Specific gaps, or —] |

## Core Utilities

| Utility | Status | Notes |
|---|---|---|
| [Utility name] | [Status] | [Key classes/pipeline steps] |

## Integrations _(if applicable)_

| Integration | Status | Notes |
|---|---|---|
| [Service] | [Status] | [Auth method, endpoints, gaps] |
```

### Status indicators

| Status | Meaning |
|---|---|
| `Built` | Fully implemented and working |
| `Partial` | Core exists but has known gaps |
| `Stubbed` | Scaffolding present, logic not wired |
| `Not started` | Planned but no code yet |
| `Dev-only` | Works with workarounds (hardcoded values, mock data) |

### Rules

- **Feature-level, not file-level.** "OCR pipeline: Built" > listing 5 file names.
- **"What's missing" is the most valuable column.** Be specific: "OCR→routing→destination not wired" not "incomplete".
- **No ticket numbers.** Status must be self-describing to a fresh session.
- **No padding.** If a section is empty, omit it. If notes add nothing, use `—`.
- **Be honest about gaps.** If something is stubbed, say so. New sessions need truth, not optimism.

## After Writing

Tell the user:
1. The file path written
2. What changed — new rows added, statuses promoted, entries enhanced
3. Count of `Partial` / `Not started` / `Stubbed` items remaining
