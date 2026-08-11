# Example: own-position-agent — the resulting record

Continues from `input.md`. Two axes considered, two different honest
outcomes — not two of the same, per the task this example demonstrates.
JSON validates against `.claude/memory/schema.json`.

## Approval Friction: omitted

No record is written for this axis. Nothing in the store resolves where
"we" stand on it — `decision-2026-09-02-budget-owner-notification-mechanism`
explicitly says the opposite: whether admin approval is removed at all
remains formally undecided. Writing a guessed value here — even a
cautious middling 3 — would be exactly the fabrication this platform's
evidence-attribution rule exists to prevent. The axis simply doesn't
appear for "Us" anywhere in this store, which is the correct, honest
outcome: a missing position, not a placeholder one.

## Budget Visibility Immediacy: scored, provisional

```json
{
  "id": "own-position-2026-09-25-budget-visibility-immediacy",
  "type": "own-position",
  "title": "Our position on Budget Visibility Immediacy is provisional and low-urgency",
  "summary": "The project's own stance is a weekly digest — a real but deliberately non-real-time signal — per decision-2026-09-02-budget-owner-notification-mechanism, which itself rests on an unvalidated assumption.",
  "status": "draft",
  "confidence": "low",
  "data": {
    "axis": "Budget Visibility Immediacy",
    "axis_low_label": "No signal to the budget owner when a new seat is added",
    "axis_high_label": "Budget owner is notified in real time when a new seat is added",
    "value": 2,
    "rationale": "decision-2026-09-02-budget-owner-notification-mechanism recommends a weekly digest to the budget owner rather than a real-time alert — a genuine, scheduled signal (ruling out the lowest value, which TeamFlo-style no-signal competitors occupy) but explicitly and deliberately not real-time, with up to a week's delay by design (ruling out anything near the highest value). That decision's own rationale states this recommendation rests on assumption-2026-08-20-budget-owner-notification-preference, which is itself unvalidated with an actual budget owner — review-agent's own example findings already flagged this exact assumption as the store's single blocking gap. This position is therefore provisional, not settled: status is draft and confidence is low specifically because of that named, unresolved assumption, not as a generic hedge."
  },
  "tags": ["self-serve-invites", "billing", "perceptual-map"],
  "sources": [],
  "related_ids": [
    "decision-2026-09-02-budget-owner-notification-mechanism",
    "assumption-2026-08-20-budget-owner-notification-preference",
    "competitor-2026-08-25-teamflo",
    "competitor-2026-08-25-huddleworks",
    "competitor-2026-08-26-rosterpilot"
  ],
  "owner": "own-position-agent",
  "created_at": "2026-09-25T13:00:00Z",
  "updated_at": "2026-09-25T13:00:00Z"
}
```

Note the `axis`, `axis_low_label`, and `axis_high_label` here are
byte-identical to the ones already on `competitor-2026-08-25-teamflo`,
`competitor-2026-08-25-huddleworks`, and
`competitor-2026-08-26-rosterpilot`'s scores for this same axis — a
future rendering surface can plot all four points (three competitors plus
this one) on one line without any reconciliation. `value: 2` sits between
Huddleworks (1, no signal at all) and RosterPilot (3, an always-current
but pull-based banner) — closer to the low end than the middle, matching
a mechanism that's a real, scheduled signal but explicitly and
deliberately not real-time.

## What's deliberately absent

No `competitor` record was written or modified. No axis was invented —
both considered here already existed on competitor scores from Task 11.
No mitigation, fix, or resolution was proposed for the unvalidated
assumption this position rests on; that stays exactly as unresolved as
`review-agent` and `strategy-agent`'s examples already left it. No chart,
map, or document was rendered — that's a future rendering surface's job,
not this one.
