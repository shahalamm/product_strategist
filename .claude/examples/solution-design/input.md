# Example: solution-design — input

This example reuses, verbatim, the 17-record `self-serve-team-invites`
memory store built up across the prior examples (`discovery-interview`,
`research-agent`, `strategy-agent`), targeting a specific decision from
it rather than inventing one.

Project slug: `self-serve-team-invites`. Target decision id:
`decision-2026-09-02-budget-owner-notification-mechanism` — the
weekly-digest recommendation from
`.claude/examples/strategy-agent/output.md`.

## Step 1 precondition check

`solution-design` reads the store and looks up the target decision, per
Step 1 of `.claude/skills/solution-design/SKILL.md`:

1. **Exists** — yes, `decision-2026-09-02-budget-owner-notification-mechanism`
   is in the store.
2. **`type` is `decision`** — yes.
3. **`status` is `"active"`** — yes.

All three clear, so the skill proceeds rather than stopping.

## Step 2: caveats already flagged on the target decision

The target decision's own `data.open_questions` (see
`.claude/examples/strategy-agent/output.md`) flags three things:

1. `assumption-2026-08-20-budget-owner-notification-preference` — the
   recommendation rests on this, and it's unvalidated. **Must be carried
   forward** into a requirement's `details`, per Step 2.
2. Digest delivery channel (email vs. in-app) is not yet decided. **This
   is exactly the kind of small implementation choice Step 4 says to
   resolve inline** — not with a new `decision` record.
3. Whether admin approval is removed at all remains formally undecided.
   This belongs to a different decision
   (`decision-2026-08-20-restrict-invite-to-full-members`'s sibling
   question) — out of scope for elaborating *this* target decision, and
   left untouched.

See `output.md` for the four requirement records this produces.
