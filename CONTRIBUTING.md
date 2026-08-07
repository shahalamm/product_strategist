# Contributing: How to Design a Skill or Agent

This document defines the conventions every future skill and agent in this
platform must follow. It exists so that capability can be added by many
different contributors (or many different Claude Code sessions) over time
without the platform fragmenting into inconsistent, one-off implementations.

No skills exist yet. This is the contract they must satisfy when they do.

## Core design rules

1. **Single responsibility.** A skill does one discovery task well (e.g.
   "synthesize interview notes into themes," "draft a competitor teardown" —
   illustrative only, not a commitment to build these). If you find yourself
   describing a skill with "and," it's probably two skills.

2. **Composable, not chained by assumption.** A skill should not assume it
   only ever runs after another specific skill. It should declare what input
   it needs and what output it produces, and work whenever those are met —
   whether the input came from a person, another skill, or memory.

3. **Stateless logic, stateful memory.** Skills should not keep their own
   private state between runs. Anything that needs to persist (an insight, a
   decision, a persona) is written to `.claude/memory/` using the shared
   schema, so any other skill or agent can read it later.

4. **Evidence-carrying outputs.** Any claim a skill produces should carry
   enough provenance (source, confidence, date) to be checked later. Don't
   silently drop that information for the sake of a cleaner-looking output.

5. **No hidden side effects.** A skill should only write to
   `.claude/outputs/` and `.claude/memory/`. It shouldn't reach into
   `templates/`, `skills/`, or `agents/` and modify them at runtime.

## Anatomy of a skill

Each skill lives in its own directory under `.claude/skills/<skill-name>/`:

```
.claude/skills/<skill-name>/
├── SKILL.md          # Required. Frontmatter + instructions.
├── references/        # Optional. Supporting docs the skill reads as-needed.
├── scripts/            # Optional. Deterministic helper scripts, if any.
└── assets/              # Optional. Static files the skill's output depends on.
```

`SKILL.md` requires YAML frontmatter with at least:

```yaml
---
name: skill-name-in-kebab-case
description: >
  One or two sentences describing what the skill does and, concretely,
  when it should trigger. This is what Claude Code matches against —
  vague descriptions cause the skill to fire at the wrong times or never.
---
```

Followed by clear, direct instructions for what the skill should do when
invoked, written the way you'd brief a capable colleague — not a marketing
description of the skill.

## Anatomy of an agent

Agent definitions live under `.claude/agents/<agent-name>.md` and declare a
focused persona with a narrow tool surface — used for delegating a specific
kind of discovery work (e.g. research, synthesis, critique) out of the main
context. An agent should be justified by a real need to parallelize work or
protect the main context window, not created by default for every skill.

## The memory contract

Any skill or agent that produces something worth remembering — an insight,
a decision, a persona, a competitor profile, a validated or invalidated
assumption — must write it as a record conforming to
`.claude/memory/schema.json`. See `.claude/memory/README.md` for the full
contract, including required fields, memory store scoping (one store file
per project), and how to reference related records.

Memory stores are one-per-project (`.claude/memory/<project-slug>.json`),
so records don't need a `project_id` to disambiguate — the store file
already scopes them.

Records have a generic shape plus an optional `data` field for
type-specific structured payloads. `data` is unconstrained for most
types, but `schema.json` uses JSON Schema `if`/`then` rules to require a
specific `data` shape for types where that shape is already load-bearing
(`decision` needs `alternatives_considered`, `recommendation`, and
`rationale`; `assumption` needs `statement` and `validation_status` — see
`.claude/memory/README.md` for full examples). A skill that writes a
`decision` or `assumption` record must populate `data` in that shape, not
just narrate the equivalent content in `details` or `summary` — those
fields are for human-readable framing, not for holding data another skill
needs to parse.

When a skill needs a different type (e.g. `persona`, `competitor`) to
carry specific required fields, add a matching `if`/`then` block to
`schema.json` at that point rather than inventing an ad hoc shape in the
skill itself — don't add structure to a type before a skill actually
needs it.

Skills should also **read** relevant memory before producing new output,
so discovery work compounds instead of repeating itself.

## Outputs and templates

- If a skill produces a document-shaped artifact (a brief, a PRD, a persona
  sheet), it should render from a template in `.claude/templates/` rather
  than inventing formatting inline. If no suitable template exists, add one.
- Generated artifacts are written to `.claude/outputs/`, organized so a human
  can find them without reading the skill that made them (see that folder's
  README for the expected layout).

## Examples are part of the skill, not an afterthought

Every new skill should ship with at least one example in
`.claude/examples/` showing a realistic input and the output it should
produce. Examples double as the informal spec reviewers check new versions
of the skill against.

## Naming

- Skills and agents: `kebab-case`, verb-first where possible
  (`synthesize-interview-notes`, not `interview-notes-tool`).
- Memory record `type` values: lowercase, singular, hyphenated
  (`feature-idea`, not `FeatureIdeas`).
- Template files: `kebab-case.md`, named after the artifact they produce
  (`competitor-teardown.md`, not `template3.md`).

## Review checklist before adding a skill

- [ ] Does exactly one thing, describable in a single sentence
- [ ] `SKILL.md` frontmatter has a specific, trigger-worthy `description`
- [ ] Declares its inputs and outputs explicitly
- [ ] Reads relevant memory before producing new output
- [ ] Writes memory-worthy findings using `.claude/memory/schema.json`
- [ ] Renders document output through a `.claude/templates/` template
- [ ] Includes at least one example under `.claude/examples/`
- [ ] Introduces no new dependency or tooling unless truly required
