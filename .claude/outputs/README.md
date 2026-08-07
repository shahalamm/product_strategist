# outputs/

Generated discovery artifacts produced by skills — briefs, PRDs, persona
sheets, competitive analyses, roadmaps, and similar documents rendered from
`.claude/templates/`.

This folder holds *results*, not logic. Nothing here should be read back
into a skill as configuration or behavior — durable, structured findings
belong in `.claude/memory/` instead, so they can be reasoned about
programmatically. Treat `outputs/` as the human-facing deliverable shelf.

## Status

Empty by design. No skills exist yet to generate outputs — this repository
is currently scaffolding only.

## Conventions (for when outputs start being generated)

- Organize by project or initiative first, then by artifact type, e.g.
  `outputs/<project-slug>/<artifact-type>/<artifact-name>.md`.
- Include a short header in generated documents noting which skill produced
  them, when, and from which memory records/sources — so an output is
  traceable back to its evidence.
- Treat this folder as disposable/regenerable where possible: if an output
  can be re-derived from `.claude/memory/`, prefer that over treating the
  file itself as the source of truth.
