# AI Product Discovery Platform

This file is the source of truth for how Claude Code should think and act in this
repository. Read it before doing any work here.

## What this project is

An AI Product Discovery Platform: a system of Claude Code **skills** and **agents**
that assist product teams through the discovery process — market research,
customer insight synthesis, competitive analysis, opportunity framing, hypothesis
validation, and early-stage artifact generation (briefs, PRDs, personas, roadmaps).

The platform is not one monolithic program. It is a growing collection of small,
composable capabilities that share a common memory and a common set of
conventions, orchestrated by Claude Code.

> **Current state:** this repository is a scaffold only. No skills, agents, or
> discovery logic have been implemented yet. Do not add discovery functionality
> until it is explicitly requested — the structure below is what future work
> builds into.

## Philosophy

1. **Evidence over opinion.** Every insight, recommendation, or artifact this
   platform produces should be traceable to a source — a document, a data point,
   a conversation, a prior decision in memory. Unsourced claims are treated as
   hypotheses, not facts, and must be labeled as such.

2. **Discovery is iterative, not a pipeline.** Real product discovery loops:
   research informs hypotheses, hypotheses get tested, results reshape research.
   Skills and agents should be designed to be re-entered and re-run, not treated
   as one-shot linear steps.

3. **Modularity over monoliths.** Capability lives in small, single-purpose
   skills and agents that can be composed, replaced, or removed independently.
   No skill should try to "do discovery" end to end — it should do one
   well-defined thing well.

4. **Shared memory, not siloed context.** Agents and skills read from and write
   to a common project memory (see `.claude/memory/`) so that work compounds
   across sessions instead of starting cold every time. Memory is structured
   and versioned, not a dumping ground.

5. **Human-in-the-loop by default.** This platform accelerates discovery; it
   does not make product decisions. Outputs are recommendations with visible
   confidence and assumptions, meant to be reviewed, challenged, and decided on
   by people.

6. **Extensible by convention, not by exception.** New skills, agents, and
   templates should be addable by following the patterns in `CONTRIBUTING.md`
   without needing to modify core structure or existing components.

7. **Build narrow, then widen.** Prefer scaffolding and validating structure
   before implementing behavior. Don't design for hypothetical future
   requirements — extend the platform when a real discovery need justifies it.

## Repository map

```
/
├── CLAUDE.md                 # This file — philosophy and operating principles
├── CONTRIBUTING.md           # How to design and add new skills
└── .claude/
    ├── skills/                # Reusable Claude Code skills (not yet implemented)
    ├── agents/                # Specialized subagent definitions (not yet implemented)
    ├── templates/             # Output templates skills/agents render into
    ├── memory/                # Shared, structured project memory + its schema
    ├── outputs/                # Generated discovery artifacts
    └── examples/                # Reference inputs/outputs for skill design and testing
```

Each folder has its own `README.md` explaining its purpose in more detail —
read the local README before adding anything to a folder.

## Operating principles for Claude Code in this repo

- **Don't implement skills or agents unless explicitly asked.** This repo may
  go through several scaffolding/planning passes before real capability is
  built. Check the request scope before writing behavior.
- **Follow `CONTRIBUTING.md`** when a skill or agent is eventually added — it
  defines the required shape (frontmatter, structure, memory contract, output
  conventions) so the platform stays consistent as it grows.
- **Respect the memory schema.** Anything written to `.claude/memory/` must
  conform to `.claude/memory/schema.json`. Don't invent parallel storage
  formats for the same kind of data.
- **Keep outputs out of core logic.** Generated artifacts belong in
  `.claude/outputs/`, not mixed into `skills/`, `agents/`, or `templates/`.
- **Prefer editing over duplicating.** If a new need looks like an existing
  skill, template, or memory type, extend it rather than forking a near-copy.
- **Keep the scaffold clean.** Don't add tooling, dependencies, or frameworks
  that aren't needed yet. Every addition should map to a real, current
  requirement.
