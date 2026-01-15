# Claude Toolkit

A collection of agents, prompts, commands, skills, etc. to use with Claude Code and other LLMs.

## Commands

- [spec-ui](/agents/spec-ui.md): Feed UI designs and it will help draft engineering specs by systematically uncovering logic, state, and dependencies. It does not look at viusal styling unless it's part of logic. It is collaborative; have a back-and-forth with it by explaining behavior and it will output a concise and prescriptive spec of functionality which can then be used to draft Jira tickets.
- [addlog](/commands/addlog.md): Use this command to add product and design decisions to a running decision log file in your codebase. Use it after discussing features with Claude and reaching conclusions. It automatically finds your decision log (usually DECISIONS.md) and appends entries with timestamps. Entries are intentionally concise (fragments, no articles) to keep the log scannable.
