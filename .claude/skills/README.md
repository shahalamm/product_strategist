# skills/

Reusable Claude Code skills that make up the platform's discovery
capabilities. Each skill is a self-contained, single-purpose unit that
Claude Code can invoke — e.g. (illustrative, none exist yet) synthesizing
research notes, drafting a competitor teardown, or scoring an opportunity.

## Status

Empty by design. No skills have been implemented yet — this repository is
currently scaffolding only.

## Structure

Each skill gets its own directory:

```
skills/<skill-name>/
├── SKILL.md          # Required: frontmatter (name, description) + instructions
├── references/         # Optional: supporting docs the skill consults
├── scripts/             # Optional: deterministic helper scripts
└── assets/               # Optional: static files the skill depends on
```

## Before adding a skill

Read `/CONTRIBUTING.md` at the repository root. It defines the required
shape of a skill, the memory contract skills must follow, and the review
checklist a new skill should satisfy before it's considered done.
