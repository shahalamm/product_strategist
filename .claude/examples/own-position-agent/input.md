# Example: own-position-agent — input

This example reuses, verbatim, the 26-record `self-serve-team-invites`
memory store as it stood after Task 11's scoring pass (see
`.claude/examples/research-agent/output.md`'s "Step 5: scoring pass"
section), including the `data.scores` now on `competitor-2026-08-25-teamflo`,
`competitor-2026-08-25-huddleworks`, and `competitor-2026-08-26-rosterpilot`.

Project slug: `self-serve-team-invites`. Store:
`.claude/memory/self-serve-team-invites.json`.

## Step A: axes found

Scanning every `competitor` record's `data.scores`, two distinct axes
exist, each used with byte-identical `axis_low_label`/`axis_high_label`
text across every competitor scored on it:

1. **Approval Friction** — `axis_low_label`: "No approval step required
   for any invite"; `axis_high_label`: "Every invite requires explicit
   approval before it completes." Scored for TeamFlo (1) and Huddleworks
   (5); RosterPilot was deliberately left unscored on this axis in the
   Task 11 example.
2. **Budget Visibility Immediacy** — `axis_low_label`: "No signal to the
   budget owner when a new seat is added"; `axis_high_label`: "Budget
   owner is notified in real time when a new seat is added." Scored for
   all three competitors (TeamFlo 5, Huddleworks 1, RosterPilot 3).

## Step B: does the store resolve "Us" on each?

**Approval Friction:** searching every `decision`, `requirement`, and
`system-impact` record for anything that decides whether admin approval
is removed at all turns up exactly one hit —
`decision-2026-09-02-budget-owner-notification-mechanism`'s own
`open_questions`, which states outright: *"Whether admin approval is
removed at all remains formally undecided in memory... this decision
addresses notification mechanism only, on the assumption approval
removal proceeds; it does not re-decide that."* Nothing resolves it —
the opposite: a record explicitly confirms it's unresolved. Per Step B,
**this axis is omitted for "Us."**

**Budget Visibility Immediacy:** the same decision's `data.recommendation`
directly addresses this axis — a weekly digest to the budget owner. So
this axis **is** scored — but that decision's own `rationale` says the
recommendation rests on `assumption-2026-08-20-budget-owner-notification-preference`,
which is unvalidated. `review-agent`'s own example
(`.claude/examples/review-agent/output.md`, Finding 1) already flagged
this exact assumption as the store's single blocking gap. Per Step B,
this position is written, but as `status: "draft"` / `confidence: "low"`,
naming that specific assumption.

See `output.md` for the resulting record.
