# Example: strategy-agent — input

This example reuses, verbatim, the full 15-record `self-serve-team-invites`
memory store built up across the prior examples — 6 records from
`.claude/examples/discovery-interview/` and 9 from
`.claude/examples/research-agent/` — rather than inventing a project for
the occasion.

Project slug: `self-serve-team-invites`. Store:
`.claude/memory/self-serve-team-invites.json`, containing:

- `insight-2026-08-18-support-escalation-origin`
- `assumption-2026-08-20-approval-rarely-blocks`
- `insight-2026-08-20-billing-tied-to-approval`
- `risk-2026-08-20-losing-budget-checkpoint`
- `decision-2026-08-20-restrict-invite-to-full-members`
- `assumption-2026-08-20-budget-owner-notification-preference`
- `research-source-2026-08-25-teamflo-no-approval`
- `research-source-2026-08-25-teamflo-notification`
- `competitor-2026-08-25-teamflo`
- `research-source-2026-08-25-huddleworks-widened-approval`
- `research-source-2026-08-25-huddleworks-flat-billing`
- `competitor-2026-08-25-huddleworks`
- `research-source-2026-08-26-rosterpilot-threshold-approval`
- `research-source-2026-08-26-rosterpilot-ambient-banner`
- `competitor-2026-08-26-rosterpilot`

## Step 1 precondition check

`strategy-agent` reads the full store and checks, per Step 1 of
`.claude/agents/strategy-agent.md`:

1. **Concrete problem/scope** — established by
   `decision-2026-08-20-restrict-invite-to-full-members` (who this applies
   to) and `insight-2026-08-18-support-escalation-origin` (the originating
   problem). Both are settled, not hedges.
2. **Enough grounding to compare real alternatives** — `risk-2026-08-20-losing-budget-checkpoint`
   (an open risk) and `assumption-2026-08-20-budget-owner-notification-preference`
   (an open assumption) both exist and are exactly the kind of thing
   options would need to address.

Both preconditions clear, so the agent proceeds rather than stopping.

## The specific question this invocation addresses

The Capability Matrix from `research-agent`'s example
(`.claude/examples/research-agent/capability-matrix.example.md`) left one
row explicitly "Not yet decided": whether and how the budget owner is kept
aware of new self-invited seats. That's the exact open question this
invocation targets — not the admin-approval question (the other "Not yet
decided" row), and not the role-scope question (already closed by
`decision-2026-08-20-restrict-invite-to-full-members`, which this agent
does not revisit).

The store also has three `competitor` records
(`competitor-2026-08-25-teamflo`, `competitor-2026-08-25-huddleworks`,
`competitor-2026-08-26-rosterpilot`) with research on exactly this
question — per Step 2 of `.claude/agents/strategy-agent.md`, the options
below must engage with them, not ignore them.

See `output.md` for the options this produces.
