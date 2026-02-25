# Claude Toolkit

A collection of agents, prompts, commands, skills, etc. to use with Claude Code and other LLMs.

## Commands

Commands are slash commands you can use directly in Claude Code.

- [spec-ui](/commands/spec-ui.md): Feed UI designs and it will help draft engineering specs by systematically uncovering logic, state, and dependencies. It does not look at visual styling unless it's part of logic. It is collaborative; have a back-and-forth with it by explaining behavior and it will output a concise and prescriptive spec of functionality which can then be used to draft Jira tickets.
- [add-log](/commands/add-log.md): Use this command to add product and design decisions to a running decision log file in your codebase. Use it after discussing features with Claude and reaching conclusions. It automatically finds your decision log (usually DECISIONS.md) and appends entries with timestamps. Entries are intentionally concise (fragments, no articles) to keep the log scannable.
- [crit-product](/commands/crit-product.md): Transforms Claude into an elite Product Manager who critiques and refines product ideas. Use it to validate product concepts, make strategic decisions, or work through feature-level details. Claude pushes back with clarifying questions and challenges assumptions to help identify what will make your product successful. Expect honest feedback.
- [merge-without-pr](/commands/merge-without-pr.md): Merge a worktree to main directly via rebase and fast-forward, without opening a pull request.

## Skills

Skills are reusable capabilities that extend Claude's behaviour for specific workflows.

- [doc-auditor](/skills/doc-auditor/SKILL.md): Audit application specifications and planning documents to surface logical gaps, inconsistencies, contradictions, missing specifications, and ambiguities. Cross-examines PRDs, technical specs, architecture docs, user stories, and API contracts to identify issues that would block implementation. Produces a prioritized audit report saved to `docs/audits/`.
- [spec-to-linear](/skills/spec-to-linear/SKILL.md): Converts project specification documents (PRDs, technical specs, database schemas, architecture docs, etc.) into structured Linear issues organized by development layer. Discovers and reads all planning documents, derives meaningful layers from the project architecture, generates actionable issues with clear dependencies, and creates them in Linear after your review and approval.
- [update-progress](/skills/update-progress/SKILL.md): Generates or updates `docs/implementation-status.md` with a feature-level snapshot of what's built, partial, stubbed, or missing. Use when starting a new session, after completing a feature, or when a new agent needs project context.
