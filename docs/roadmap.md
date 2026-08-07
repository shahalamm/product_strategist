# Build order

The platform is built one skill/agent at a time, narrow before wide, per
`CLAUDE.md`'s philosophy. This is a build-order list, not a re-plan — see
`CLAUDE.md` for the project's actual philosophy and principles, and
`CONTRIBUTING.md` for how each of these is required to be shaped.

## Built

1. **`discovery-interview`** (skill) — interrogates a rough,
   underspecified product idea one question at a time, probing vague
   answers before accepting them, and writes `assumption`/`decision`/
   `insight`/`risk` records to the project's memory store.
2. **`review-agent`** (agent) — reads a project's memory store fresh and
   surfaces specific, severity-ranked findings on whether it's defensible
   enough to move toward research or strategy (high-leverage unvalidated
   assumptions, foregone-conclusion decisions, unacknowledged risks,
   contradictions). Reports only — never edits, never gates.
3. **`research-agent`** (agent) — given a project's already-scoped memory
   store, researches competitors and market context specific to that
   scope, writes `research-source`/`competitor` records, and renders a
   Capability Matrix comparing the project's own stance to what was
   found. Refuses to research a broad category if scope isn't established
   yet.
4. **`strategy-agent`** (agent) — given an already-grounded open
   risk/assumption, generates 2-4 solution options with a structured
   impact assessment each, engages with any existing competitor research
   rather than ignoring it, and recommends exactly one via a single
   tagged `decision` record with a traceable rationale. Doesn't research
   or design — reads memory, writes one decision.
5. **`solution-design`** (skill) — given a specific active `decision` id
   (not only strategy recommendations — a discovery-interview scope
   decision qualifies too), elaborates it into concrete, testable
   `requirement` records. Never re-litigates the target decision and never
   writes a new `decision` for small implementation choices — those get
   resolved inline in a requirement's `details` instead. No output
   document; the requirement records are the deliverable.

## Still to come

- **systems-impact** — assesses what a proposed design touches downstream
  (technical, operational, org). Not built yet.
- **documentation-agent** — renders the artifacts this platform is meant
  to produce (briefs, PRDs, personas, roadmaps) from accumulated memory.
  Not built yet.

Each of these, when built, follows the same pattern as the ones above:
scaffolded first if needed, then implemented only when explicitly
requested, per `CONTRIBUTING.md`.
