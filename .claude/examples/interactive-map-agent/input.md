# Example: interactive-map-agent — input

This example reuses, verbatim, the 27-record `self-serve-team-invites`
memory store as it stood after `own-position-agent`'s example (Task 12):
the 26 records from every prior example, plus
`own-position-2026-09-25-budget-visibility-immediacy`.

Project slug: `self-serve-team-invites`.

## Step 1: axes and who's plottable on each

Exactly two distinct axes exist anywhere in this store's `competitor`
scores and `own-position` records — no others have ever been established:

| Entity | Approval Friction | Budget Visibility Immediacy |
|---|---|---|
| TeamFlo (`competitor-2026-08-25-teamflo`) | 1 | 5 |
| Huddleworks (`competitor-2026-08-25-huddleworks`) | 5 | 1 |
| RosterPilot (`competitor-2026-08-26-rosterpilot`) | — | 3 |
| Us (`own-position-2026-09-25-budget-visibility-immediacy`, `status: draft`) | — | 2 |

With only two axes total, there is exactly **one** possible axis-pair:
**Approval Friction × Budget Visibility Immediacy**.

Checking who's plottable on *that specific pair* (needs a real score on
**both** axes):

- **TeamFlo** — has both. Plottable.
- **Huddleworks** — has both. Plottable.
- **RosterPilot** — has Budget Visibility Immediacy only (Approval
  Friction was deliberately left unscored in `research-agent`'s Task 11
  example — the evidence didn't support a confident number). **Not
  plottable on this pair.**
- **Us** — has Budget Visibility Immediacy only (Approval Friction was
  omitted by `own-position-agent`'s Task 12 example — nothing in the
  store resolves whether admin approval is removed at all). **Not
  plottable on this pair.**

Two plottable entities (TeamFlo, Huddleworks) meets the "at least two"
threshold from Step 1 of `.claude/agents/interactive-map-agent.md`, so
this one pair **is** rendered — with exactly two points, and RosterPilot
and Us named as excluded rather than silently dropped or forced on.

No other axis-pair exists to consider, since there are only two axes in
total.

See `output.md` for the rendering decisions, and
`interactive-map.example.html` for the actual rendered artifact.
