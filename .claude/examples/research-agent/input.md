# Example: research-agent — input

This example reuses, verbatim, the six-record `self-serve-team-invites`
memory store already used by the `discovery-interview` and `review-agent`
examples, rather than inventing a project for the occasion — see
`.claude/examples/discovery-interview/` for how those six records came to
be, and `.claude/examples/review-agent/input.md` for the same store laid
out in full.

Project slug: `self-serve-team-invites`. Store:
`.claude/memory/self-serve-team-invites.json`, containing:

- `insight-2026-08-18-support-escalation-origin`
- `assumption-2026-08-20-approval-rarely-blocks`
- `insight-2026-08-20-billing-tied-to-approval`
- `risk-2026-08-20-losing-budget-checkpoint`
- `decision-2026-08-20-restrict-invite-to-full-members`
- `assumption-2026-08-20-budget-owner-notification-preference`

## Step 1 scope check (before researching anything)

`research-agent` reads this store and checks for a concrete problem and a
concrete user/scope, per its own SKILL.md-equivalent (Step 1 of
`.claude/agents/research-agent.md`):

- **Problem** — concrete: `insight-2026-08-18-support-escalation-origin`
  ties the idea to a specific incident (a 3-day onboarding block caused by
  a single unavailable admin).
- **User/scope** — concrete: `decision-2026-08-20-restrict-invite-to-full-members`
  settles who this applies to (full members only; guests and contractors
  excluded) — this is a `decision`, not a hedge, so it clears the bar.

Both are concrete, so the agent proceeds rather than stopping to recommend
further `discovery-interview` work. It forms this narrow research
question from the scope:

> How do other B2B SaaS products let full team members self-invite new
> teammates — do they gate it behind any approval step, and if not, how
> (if at all) do they keep whoever owns the budget aware of new seats
> being added?

Note what this question deliberately does **not** ask: it doesn't research
"team collaboration software" as a category, and it doesn't ask about
guest/contractor invites, because `decision-2026-08-20-restrict-invite-to-full-members`
already ruled that scope out.

See `output.md` for the research this question produces.
