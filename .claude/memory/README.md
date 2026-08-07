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

## Scoping: one memory store per project

**Decision:** memory stores are one-per-project, not shared across
projects. Each product discovery engagement gets its own store file:

```
.claude/memory/<project-slug>.json
```

e.g. `.claude/memory/teams-plan-pricing.json`,
`.claude/memory/onboarding-redesign.json` — each a self-contained store
conforming to `schema.json` (a `version` plus that project's `records`).

Because the file itself already identifies the project, records do not
need to carry a `project_id` to disambiguate which project they belong
to — a skill reading `.claude/memory/onboarding-redesign.json` already
knows every record in it is scoped to that project. The `project_id`
field still exists in `schema.json`, but it is **unused under this
scoping** — it's kept only in case a future need for a single
multi-project store arises, and should not be populated unless that
change is explicitly adopted (which would also require updating this
section). Skills and agents should not read or write `project_id` today.

## Why `decision` and `assumption` require a `data` shape, and other types don't

`schema.json` enforces a required structured shape (via `data`) only for
`decision` and `assumption` records right now. That's not an oversight —
it's because those are the two record types with an obvious, stable shape
that's already in active use for how this platform reasons about
discovery (a decision needs its alternatives and rationale to be useful
later; an assumption needs to be a falsifiable statement with a
validation status, or it isn't actionable).

The other types (`insight`, `persona`, `competitor`, `market-signal`,
`opportunity`, `risk`, `feature-idea`, `research-source`) stay generic —
`data` is optional and unconstrained for them — **on purpose**: we don't
yet have a skill that produces or consumes those types, so we don't yet
know their real required shape. Adding structure speculatively risks
guessing wrong and having to break records already written. When a skill
is built that needs one of these types to carry specific fields, add a
matching `if`/`then` block to `schema.json` at that point — see
`CONTRIBUTING.md`'s memory contract section.

## Example records

Two fully-populated examples showing correct use of `data` for the two
structured types. Both validate against `schema.json`.

### `decision`

```json
{
  "id": "decision-2026-08-15-teams-plan-pricing-model",
  "type": "decision",
  "title": "Adopt seat-based pricing for the Teams plan launch",
  "summary": "Chose seat-based pricing over usage-based or flat-rate for the upcoming Teams plan, based on competitor benchmarking and finance modeling.",
  "status": "active",
  "confidence": "medium",
  "data": {
    "alternatives_considered": [
      {
        "option": "Seat-based pricing ($18/seat/month)",
        "pros": ["Predictable revenue", "Matches how the 3 closest competitors price", "Simple for sales to explain"],
        "cons": ["Penalizes teams with many light/occasional users"]
      },
      {
        "option": "Usage-based pricing (per workspace action)",
        "pros": ["Aligns cost to value realized"],
        "cons": ["Unpredictable for customers' budgeting", "Requires metering infrastructure we don't have yet"],
        "rejected_because": "No usage metering exists today; would delay launch by a full quarter to build it."
      },
      {
        "option": "Flat-rate per workspace",
        "pros": ["Simplest possible pricing"],
        "cons": ["Leaves revenue on the table for large teams", "No natural expansion motion"],
        "rejected_because": "Finance modeling showed it undercharges teams above 10 seats, our target segment."
      }
    ],
    "recommendation": "Launch the Teams plan at $18/seat/month, billed monthly or annually.",
    "rationale": "Seat-based pricing is fastest to ship, matches customer mental models from competitor products, and finance projects it captures more revenue than flat-rate for our target segment (10-50 seat teams) without the metering work usage-based pricing would require.",
    "open_questions": [
      "Do we need a minimum seat count to avoid single-seat 'Teams' signups undercutting the individual plan?",
      "Annual discount percentage is not yet finalized with finance."
    ]
  },
  "tags": ["pricing", "teams-plan"],
  "sources": [
    { "label": "Competitor pricing benchmark - illustrative example", "reference": "n/a - placeholder example, not a real source" }
  ],
  "related_ids": [],
  "owner": "example",
  "created_at": "2026-08-15T00:00:00Z",
  "updated_at": "2026-08-15T00:00:00Z"
}
```

### Invalid: `decision` missing `data`

`schema.json`'s `if`/`then` rule for `decision` includes
`"required": ["data"]`, so a `decision` record that omits `data` entirely
— not just one with a malformed `data` — fails validation. The same rule
applies to `assumption`. This record is otherwise well-formed but is
**invalid** against `schema.json`, with the error
`'data' is a required property`:

```json
{
  "id": "decision-2026-08-15-teams-plan-pricing-model",
  "type": "decision",
  "title": "Adopt seat-based pricing for the Teams plan launch",
  "summary": "Chose seat-based pricing over usage-based or flat-rate for the upcoming Teams plan, based on competitor benchmarking and finance modeling.",
  "status": "active",
  "confidence": "medium",
  "tags": ["pricing", "teams-plan"],
  "sources": [],
  "related_ids": [],
  "owner": "example",
  "created_at": "2026-08-15T00:00:00Z",
  "updated_at": "2026-08-15T00:00:00Z"
}
```

A skill that writes a `decision` record without a `data` object (e.g.
narrating the rationale only in `summary`/`details`) will fail validation
against `schema.json`, by design — see "Why `decision` and `assumption`
require a `data` shape" above.

### `assumption`

```json
{
  "id": "assumption-2026-08-15-onboarding-support-load",
  "type": "assumption",
  "title": "Self-serve onboarding won't spike support ticket volume",
  "summary": "We're assuming the redesigned self-serve onboarding flow keeps support load manageable without a dedicated onboarding specialist.",
  "status": "draft",
  "confidence": "low",
  "data": {
    "statement": "Support teams handle fewer than 5 tickets/week related to self-serve onboarding once the new flow ships.",
    "validation_status": "unvalidated",
    "impact_if_wrong": "If ticket volume is materially higher, we'd need to staff a dedicated onboarding support rotation before general availability, delaying launch by 2-3 weeks."
  },
  "tags": ["onboarding", "support"],
  "sources": [],
  "related_ids": ["insight-2026-08-07-onboarding-friction"],
  "owner": "example",
  "created_at": "2026-08-15T00:00:00Z",
  "updated_at": "2026-08-15T00:00:00Z"
}
```

## Conventions (for when memory starts being written)

- One store file per project (see scoping section above) — don't split a
  single project's records across multiple files without a real need
  (size, ownership, access pattern) that justifies it.
- Every record requires `id`, `type`, `title`, `summary`, `status`,
  `confidence`, `created_at`, and `updated_at` at minimum — see
  `schema.json` for the full field list and allowed values. `decision` and
  `assumption` records additionally require a `data` object with a
  specific shape — see the examples above and `schema.json`'s `allOf`
  rules.
- Use `related_ids` to link records instead of duplicating content across
  them (e.g. link a `feature-idea` to the `insight` records that motivated
  it, or an `assumption` to the `insight` that raised it, as in the
  example above).
- Treat memory as append-and-update, not delete-and-forget: prefer setting
  `status` to `deprecated`/`invalidated` over removing a record, so the
  history of what was believed and why stays intact.

See `/CONTRIBUTING.md` for the full memory contract skills and agents must
follow.
