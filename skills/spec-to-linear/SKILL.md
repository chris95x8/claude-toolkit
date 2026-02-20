---
name: spec-to-linear
description: Converts project specification documents (PRDs, technical specs, database schemas, architecture docs, etc.) into structured Linear issues organized by development layer. Use when the user says "create linear issues from my specs", "split my docs into linear issues", or runs /spec-to-linear. Produces a review document first, then creates issues via the Linear CLI after user approval.
---

# Spec to Linear

Reads your project's planning documents, splits all the work into manageable Linear issues organized by layer, writes a plan for you to review, and then creates the issues in Linear after your approval.

---

## Workflow

### Step 1: Discover Specification Documents

**If the user specifies files or folders**, use those as the scope.

**If the user does not specify**, auto-discover by scanning the repository for planning and spec documents in:
- `docs/`, `doc/`, `documentation/`
- `specs/`, `spec/`, `specifications/`
- `plans/`, `plan/`, `design/`, `designs/`
- `architecture/`, `arch/`, `requirements/`
- Root-level files like `README.md`, `SPEC.md`, `PLAN.md`, `ARCHITECTURE.md`

Use Glob to find `.md`, `.txt`, `.rst`, `.mdx` files. Exclude source code, generated files (lock files, build artifacts), and any existing `docs/linear-plan.md`.

List discovered files and confirm scope with the user before continuing.

---

### Step 2: Read and Understand All Documents

Read every file in scope. As you read, build a complete picture of:
- What is being built (product, features, user flows)
- Technical architecture (backend, frontend, services, data models)
- Third-party dependencies and integrations
- Any human setup steps required (accounts, external services, design assets)
- Implicit ordering constraints (e.g. auth before protected routes, schema before API)

Do not begin generating issues until all documents have been read.

---

### Step 3: Define Layers

Derive layers from the actual product you are reading about. Do not use a fixed set of layer names — infer what groupings make sense for this specific project based on its architecture, tech stack, and features.

See `references/layer-guide.md` for principles on how to identify and name layers. Every layer must have a short label (e.g. `Layer: [Name]`) used as a Linear Label.

---

### Step 4: Generate Issues

For each layer, generate a list of issues following these rules:

**One executor per issue**
- Each issue is done by either a human OR an agent — never both in the same issue.
- Human work: creating accounts, setting up Xcode projects, providing Figma designs, configuring third-party dashboards, purchasing domains.
- Agent work: writing code, implementing features, configuring files, writing schemas, building UI components.

**Issue content**
- Title: concise and actionable (imperative verb, e.g. "Implement user authentication endpoint")
- Description: short and direct — 3–6 bullet points or sentences. Enough for an agent to start without questions, but no padding.
  - What to build (one sentence)
  - Key constraints or decisions already made
  - Doc references (e.g. "See `docs/schema.md` → Users table")
  - If Figma designs are required: state it explicitly
- Dependencies: list issue titles that must be done first
- Executor: `[Human]` or `[Agent]`
- Label: the layer name

**Each issue should map to one pull request.** This is the primary sizing heuristic: if an agent would naturally open two separate PRs to complete the work, split it into two issues. If it would all land in one PR, keep it as one issue.

In practice this means:
- One issue adds one feature, fixes one bug, or implements one coherent slice of the system
- An issue that touches unrelated files or subsystems is a sign it should be split
- An issue that is "just a few lines" is fine — small PRs are good
- If you're unsure, ask: "Could this be reviewed and merged independently?" If yes, it's a valid issue.

**Do NOT assign issue numbers** — Linear auto-assigns these.

See `references/issue-templates.md` for examples of well-formed issues.

---

### Step 5: Write the Plan Document

Save the full plan to `docs/linear-plan.md` using this structure:

```markdown
# Linear Issue Plan

## Layer Summary

| Layer Label | # Issues | Description |
|-------------|----------|-------------|
| Layer: Backend Setup | 3 | ... |
| Layer: Frontend Shell | 2 | ... |
| ... | | |

---

## [Layer Label]

### [Issue Title] · [Human] or [Agent]

**Description:**
[Full issue description with doc references]

**Dependencies:** [None] or [List of issue titles that must be done first]

**Docs referenced:** [List of relevant files]

---

[Repeat for each issue]
```

End the document with:

```
---
Review this plan. When you are ready, confirm to proceed and the issues will be created in Linear.
```

After saving, tell the user the plan is ready at `docs/linear-plan.md` and ask them to review it and confirm before you create the issues.

---

### Step 6: Wait for User Confirmation

**Do not create any Linear issues until the user explicitly confirms.** They may want to edit the plan, remove issues, rename things, or adjust dependencies.

If the user requests changes, update `docs/linear-plan.md` and show them the diff before asking again.

---

### Step 7: Create Issues in Linear

Once the user confirms, use the `linear-cli` skill to create all issues.

For each issue:
1. Create it with the title, description, and label
2. Note the created issue's identifier (e.g. `ENG-42`) so you can cross-reference dependencies
3. After all issues are created, update descriptions of dependent issues to reference the actual identifiers (e.g. "Depends on ENG-42")

Create issues in layer order — complete all issues in Layer 1 before moving to Layer 2, and so on, so dependency identifiers are available when needed.

After all issues are created, print a summary table:

```
| Issue ID | Title | Layer | Executor |
|----------|-------|-------|----------|
| ENG-12   | ...   | ...   | Agent    |
| ENG-13   | ...   | ...   | Human    |
```

---

## Resources

| Resource | Purpose |
|----------|---------|
| `references/layer-guide.md` | Common layer patterns and when to use them. Load during Step 3. |
| `references/issue-templates.md` | Examples of well-formed human and agent issues. Load during Step 4. |
