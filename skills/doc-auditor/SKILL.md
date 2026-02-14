---
name: doc-auditor
description: This skill should be used when the user requests an audit of application specifications, planning documents, or implementation details. It cross-examines multiple planning and documentation files (PRDs, technical specs, architecture docs, user stories, API contracts, etc.) to surface logical gaps, inconsistencies, contradictions, missing specifications, ambiguities, and other issues that would block or impede a human engineer or agent from building the software effectively. Issues are prioritized and accompanied by suggested resolutions, and the final audit is saved to docs/audits/.
---

# Doc Auditor

## Overview

Systematically audit one or more specification or planning documents to identify every issue that would make it harder for an engineer or agent to implement the software as intended. Produce a prioritized, actionable audit report saved to `docs/audits/`.

This skill does **not** audit code — only planning and documentation artifacts.

---

## Workflow

### Step 1: Discover Documents to Audit

**If the user specifies files or folders**, use those as the audit scope.

**If the user does not specify**, auto-discover relevant documents by scanning the repository for:
- `docs/`, `doc/`, `documentation/`
- `specs/`, `spec/`, `specifications/`
- `plans/`, `plan/`
- `design/`, `designs/`
- `architecture/`, `arch/`
- `requirements/`, `prd/`, `rfcs/`, `adr/`
- Root-level markdown files that appear to be planning documents (e.g., `README.md`, `SPEC.md`, `PLAN.md`, `ARCHITECTURE.md`)

Use Glob to find `.md`, `.txt`, `.rst`, and `.mdx` files in these locations. Surface the discovered file list to the user and confirm scope before proceeding, especially if the list is large (>10 files). Allow the user to narrow or expand the scope.

**Exclude from audit:**
- Source code files
- Generated files (lock files, build artifacts)
- Existing audit documents in `docs/audits/`
- Changelog and version history files

---

### Step 2: Read and Cross-Examine All Documents

Read every file in the confirmed audit scope. As you read:

- Build a mental map of the system being described: its components, data flows, user roles, features, and constraints
- Note every claim, requirement, decision, and assumption
- Track terminology as it appears across documents — flag when the same word is used differently

Do not begin writing the audit until all documents have been read. Cross-examination is only meaningful once the full picture is available.

---

### Step 3: Apply Audit Criteria

Apply every category from `references/audit-criteria.md` to the documents. Load that file now and work through each category methodically:

1. Logical Gaps
2. Inconsistencies and Contradictions
3. Missing Specifications
4. Ambiguities
5. Critical Issues (Blockers)
6. Dependency and Sequencing Issues
7. Scope and Completeness Issues

For each issue found:
- Record which document(s) and section(s) it appears in
- Assign a priority tier (P0–P3) using the criteria in the reference file
- Draft 2–3 concrete resolution options

Aim for completeness over speed. A thorough audit that catches more issues is more valuable than a fast one that misses things.

---

### Step 4: Produce the Audit Document

Use `references/output-template.md` as the format for the audit report. Load the template and fill it in completely:

- Write an executive summary that gives the team an honest, high-level assessment
- List all issues ordered by priority (P0 first), then by affected area within the same tier
- For each issue: explain what is wrong, why it matters, and provide 2–3 specific resolution options
- Include a "Strengths" section — note what the documentation does well
- End with a "Recommended Next Steps" section with concrete, ordered actions

---

### Step 5: Save the Audit Report

Save the completed audit document to:

```
docs/audits/spec-audit-YYYY-MM-DD.md
```

Use today's date. If `docs/audits/` does not exist, create it.

If there is a natural feature or module name for the audit scope (e.g., the user said "audit the payments spec"), use a more specific filename:

```
docs/audits/payments-spec-audit-YYYY-MM-DD.md
```

After saving, report the file path to the user and provide a brief inline summary of the P0 and P1 issues so the most critical findings are immediately visible without opening the file.

---

## Resources

| Resource | Purpose |
|----------|---------|
| `references/audit-criteria.md` | Detailed checklist of all audit categories and priority tier definitions. Load this during Step 3. |
| `references/output-template.md` | Full template for the audit document. Load this during Step 4. |
