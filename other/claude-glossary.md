# Glossary

## CLAUDE.md

- Can be used to describe what the project is about and how to write code (conventions, patterns, stack info).
- Examples: use X library, don’t use Y classes, use SwiftUI for frontend, do not care about pre iOS26 compatibility, use Z file for architecture context, name things like so.
- Loaded into EVERY conversation/agent.

## Agents (or subagents)

- Used to automate step-by-step workflows.
- Configured as a role / persona that runs autonomously.
- Examples: review this code from a security POV and document all findings in a document formatted by high, medium, and low severity.
- Can invoke commands (if instructed to) or skills (if deemed necessary by the agent). It’s an orchestrator.
- Hyperfocused: Subagents start from a completely fresh context, all the info they need is in the instructions and input.
- Used when you have a major work piece broken down into pieces, a subagent for each piece.
- Because they’re hyperfocused, they can complete their work with inferior models, e.g. Sonnet instead of Opus.
- Because they do the work in a separate context, their output can be used by the main agent without polluting its context with the content of the subagent’s work.

## Slash commands

- Used to invoke a prompt manually at your own time. You’re telling Claude to do something with one word rather than a full paragraph of explanation. It’s a shortcut for a prompt.
- Meant for repetitive, single-shot tasks. Can be used to start workflows and agents.
- Examples
  - Committing to git with a certain process (e.g. merging to a staging environment and syncing)
  - /refine-ideas (asks Claude to act as a PM and refine an idea with you)
  - /compound (starts subagent that analyzes context, finds solution, finds the right doc for it, writes solution in the doc)
  - /report-bug (creates a Jira issue with your description)

## Skills

- Skills are pre-built abilities. They’re packages of knowledge with instructions, best practices, examples, and specific guidance for a task. Loading a skill is like when Kung Fu was uploaded into Neo’s brain. They’re meant to add knowledge to the current conversation (and produce an output if asked to).
- Examples
  - Brand Guidelines: hosts all brand variables and patterns, can be used by agents to build.
  - Document Suite: Makes Claude good at Word/Excel/PowerPoint/PDF. Not just reading them but actually creating proper docs with formatting, formulas, all that.
  - iOS Patterns: Load knowledge about iOS common patterns.
  - Landing Page: Load knowledge about designing landing pages and builds one for you when you need one.
  - Personas: Load knowledge about user personas for a product discussion.
- Skills don’t need to know why they’re being used. They’re modular and reusable building blocks that can be used by agents.
- Works similarly to a slash command, but meant for Claude to invoke when it understands that your prompt relates to one of the loaded skills.
- They don’t pollute your main context. Claude loads their description only to save context. The body of the Skill is loaded only when Claude deems it necessary.
- Some commands can be skills. To decide whether they need to be a skill, ask yourself if agents are meant to use the knowledge when necessary. If so, make it a skill, as skills are portable. If a back and forth is part of the process, then it should probably be a command.

## Tools

- Tools are capabilities that apps expose. E.g. Github includes the capability of creating a PR, and it exposes that through a CLI command. Gmail has the capability of reading an email, and it exposes that through an API endpoint. MacOS has the capability of opening a file, and it exposes that through a system call.
- LLMs have knowledge up to their training cut-off date. To retrieve something like the current weather, they need to perform a tool call. An app like AccuWeather allows LLM to call its tools via an MCP server they create, which exposes the tools to the LLM and the LLM can then decide which ones to call.

## MCP

- An abstraction layer between an LLM and existing tools.
- An MCP server describes and exposes an app’s tools. An MCP host validates the requests, runs the tools, and talks to the model to return the output.
- MCP offloads cognitive load to the model, but you sacrifice context. If you know exactly what you need and how to do it, use CLI instead.

## CLI

- An interface for a binary/executable installed locally on your machine. E.g. Git CLI allows you to type `git pull-request create` in the terminal to call the pull-request tool it exposes. Linear CLI allows you to pull data about a specific issue.
- Better to use CLI instead of MCP when you know exactly what you need to do instead of having an LLM interpret your query. MCPs are context heavy whereas CLIs can be run on the terminal directly, bypassing LLMs. E.g. instead of asking Claude to pull an issue’s description from Linear using its MCP, using the terminal to pull the data through Linear’s CLI tool instead.
- Claude Code can trigger real CLI commands on your machine. So if you type `gh pr create` it will directly call that tool instead of going through the MCP abstraction.
- You can create a CLI for an app by wrapping its API calls in custom commands.

## Hooks

- Hooks in Claude Code are custom, automated triggers that run shell commands at specific points during its operation.
- Examples: Automatically format code with a PostToolUse hook, enforce tests and quality checks with PostToolUse to make sure nothing was broken, custom notifications when the agent is done working, create a temporary commit before tool use or commit after tool use.
