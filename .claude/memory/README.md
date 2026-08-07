# memory/

Shared, structured project memory. This is how discovery work compounds
across sessions and skills instead of starting cold every time — an
insight found by one skill today should be readable by a different skill
next week without re-deriving it.

## Status

Empty of records by design. Only the schema exists so far — this repository
is currently scaffolding only; no skills exist yet to populate memory.

## What goes here

Anything worth remembering that resulted from discovery work: insights,
decisions, validated/invalidated assumptions, personas, competitor
profiles, market signals, opportunities, risks, feature ideas, and the
research sources behind them. See `schema.json` for the exact shape every
record must conform to.

What does **not** go here: generated documents/artifacts (those belong in
`.claude/outputs/`), and skill/agent logic or configuration (those belong
in `.claude/skills/` and `.claude/agents/`).

## Files

- `schema.json` — JSON Schema for a single memory record, and for the
  memory store file that holds a collection of them. This is the contract
  every skill and agent must write to and can rely on when reading.

## Conventions (for when memory starts being written)

- One JSON file per logical collection is fine to start
  (e.g. `memory/discovery.json`) — split further only when a real need
  (size, ownership, access pattern) justifies it.
- Every record requires `id`, `type`, `title`, `summary`, `status`,
  `confidence`, `created_at`, and `updated_at` at minimum — see
  `schema.json` for the full field list and allowed values.
- Use `related_ids` to link records instead of duplicating content across
  them (e.g. link a `feature-idea` to the `insight` records that motivated
  it).
- Treat memory as append-and-update, not delete-and-forget: prefer setting
  `status` to `deprecated`/`invalidated` over removing a record, so the
  history of what was believed and why stays intact.

See `/CONTRIBUTING.md` for the full memory contract skills and agents must
follow.
