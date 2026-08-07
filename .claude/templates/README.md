# templates/

Reusable output templates that skills and agents render discovery artifacts
into — e.g. (illustrative, none exist yet) a product brief template, a
persona sheet template, a competitor teardown template.

Templates exist so that every skill producing the same kind of artifact
produces it in the same shape, instead of each skill inventing its own
formatting. If a new skill needs a document-shaped output and no suitable
template exists, add one here rather than formatting inline in the skill.

## Status

Empty by design. No templates have been added yet — this repository is
currently scaffolding only.

## Conventions

- File names are `kebab-case.md`, named after the artifact they produce
  (e.g. `competitor-teardown.md`, not `template3.md`).
- A template defines structure and placeholders, not example content —
  example filled-in content belongs in `.claude/examples/`.
- A template should be renderable using data available from
  `.claude/memory/` records plus whatever a skill gathers at run time.

See `/CONTRIBUTING.md` for how templates fit into the skill contract.
