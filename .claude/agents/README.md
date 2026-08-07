# agents/

Specialized subagent definitions used to delegate focused discovery work out
of the main Claude Code context — e.g. (illustrative, none exist yet) a
research agent that gathers and summarizes market data, or a critique agent
that stress-tests a hypothesis.

## Status

Empty by design. No agents have been implemented yet — this repository is
currently scaffolding only.

## Structure

Each agent is defined as a single Markdown file with YAML frontmatter:

```
agents/<agent-name>.md
```

```yaml
---
name: agent-name-in-kebab-case
description: >
  What this agent is for and, concretely, when it should be used instead of
  handling the task directly or with a different agent.
tools: Read, Grep, Glob   # the narrow tool surface this agent actually needs
---
```

## When to add an agent (vs. a skill)

An agent is justified when a task genuinely benefits from being delegated
out of the main context — for parallelizable research, or to protect the
main conversation from a large volume of intermediate output. It is not a
default wrapper for every skill. See `/CONTRIBUTING.md` for the full
guidance on agent design.
