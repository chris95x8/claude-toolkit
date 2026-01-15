# Claude Toolkit

A collection of agents, prompts, commands, skills, etc. to use with Claude Code and other LLMs.

## Commands

- [spec-ui](/agents/spec-ui.md): Feed UI designs and it will help draft engineering specs by systematically uncovering logic, state, and dependencies. It does not look at viusal styling unless it's part of logic. It is collaborative; have a back-and-forth with it by explaining behavior and it will output a concise and prescriptive spec of functionality which can then be used to draft Jira tickets.
- [addlog](/commands/addlog.md): Use this command to add product and design decisions to a running decision log file in your codebase. Use it after discussing features with Claude and reaching conclusions. It automatically finds your decision log (usually DECISIONS.md) and appends entries with timestamps. Entries are intentionally concise (fragments, no articles) to keep the log scannable.
- [crit-product](/commands/crit-product.md): This command transforms Claude into an elite Product Manager who critiques and refines product ideas. Use it to validate product concepts, make strategic decisions, or work through feature-level details like button placement and data collection. Claude pushes back with clarifying questions and challenges assumptions to help you identify what will make your product successful. It operates across all scales—from market positioning to micro-interactions. Expect honest feedback: if an idea isn't viable, you'll hear it directly.
