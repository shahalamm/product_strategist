# Example: systems-impact — input

This example reuses, verbatim, the 21-record `self-serve-team-invites`
memory store built up across the prior examples
(`discovery-interview`, `research-agent`, `strategy-agent`,
`solution-design`), targeting the same decision `solution-design`'s
example elaborated.

Project slug: `self-serve-team-invites`. Target decision id:
`decision-2026-09-02-budget-owner-notification-mechanism`.

## Step 1 precondition check

`systems-impact` reads the store and checks, per Step 1 of
`.claude/agents/systems-impact.md`:

1. **Target decision exists, `type: decision`, `status: active`** — yes
   (same check `solution-design` ran before it).
2. **At least one `requirement` record with a matching
   `elaborates_decision_id`** — yes, four:
   - `requirement-2026-09-05-digest-content-and-trigger`
   - `requirement-2026-09-05-digest-delivery-channel`
   - `requirement-2026-09-05-digest-cadence`
   - `requirement-2026-09-05-digest-running-total-and-cost`

Both conditions clear, so the agent proceeds. (Had `solution-design` not
been run yet, there'd be zero matching requirements here, and this agent
would stop and say to run `solution-design` first rather than guess
impact from the decision's intent alone.)

See `output.md` for the impact findings this produces.
