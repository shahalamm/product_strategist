# templates/

Reusable output templates that skills and agents render discovery artifacts
into — e.g. (illustrative, none exist yet) a product brief template, a
persona sheet template, a competitor teardown template.

Templates exist so that every skill producing the same kind of artifact
produces it in the same shape, instead of each skill inventing its own
formatting. If a new skill needs a document-shaped output and no suitable
template exists, add one here rather than formatting inline in the skill.

## Status

Three templates exist:

- `capability-matrix.md`, used by `research-agent`
  (`.claude/agents/research-agent.md`) to render a comparison of the
  project's own stance against researched competitors. See
  `.claude/examples/research-agent/` for a filled-in example.
- `product-decision-document.md`, used by `documentation-agent`
  (`.claude/agents/documentation-agent.md`) to render a project's entire
  memory store into a fixed 35-section document, with an explicit
  "Not available" line for every section the store can't support. See
  `.claude/examples/documentation-agent/` for a filled-in example.
- `interactive-map.html`, used by `interactive-map-agent`
  (`.claude/agents/interactive-map-agent.md`) to render competitor and
  own-position scores as an interactive Chart.js scatter map — a
  self-contained HTML file, not Markdown, since the artifact itself is
  interactive. See `.claude/examples/interactive-map-agent/` for a
  filled-in example, verified to actually run in a browser, not just
  parse.

## Conventions

- File names are `kebab-case`, named after the artifact they produce
  (e.g. `competitor-teardown.md`, not `template3.md`) — `.md` for prose
  documents, `.html` for the rare case where the artifact itself needs to
  be an interactive page rather than a document Markdown can express.
- A template defines structure and placeholders, not example content —
  example filled-in content belongs in `.claude/examples/`.
- A template should be renderable using data available from
  `.claude/memory/` records plus whatever a skill gathers at run time.

See `/CONTRIBUTING.md` for how templates fit into the skill contract.
