# Example: review-agent — findings

Continues from `input.md`. Walks through all four checks from
`.claude/agents/review-agent.md` against the `self-serve-team-invites`
store, showing the reference graph reasoning behind each result — including
two checks that clear cleanly, to show the agent doesn't pad its output
with findings that aren't there.

## Reference graph (built in Step 1)

| record | outbound `related_ids` | inbound references |
|---|---|---|
| `insight-2026-08-18-support-escalation-origin` | — | `assumption-2026-08-20-approval-rarely-blocks`, `decision-2026-08-20-restrict-invite-to-full-members` |
| `assumption-2026-08-20-approval-rarely-blocks` | `insight-2026-08-18-support-escalation-origin` | `insight-2026-08-20-billing-tied-to-approval`, `assumption-2026-08-20-budget-owner-notification-preference` |
| `insight-2026-08-20-billing-tied-to-approval` | `assumption-2026-08-20-approval-rarely-blocks` | `risk-2026-08-20-losing-budget-checkpoint` |
| `risk-2026-08-20-losing-budget-checkpoint` | `insight-2026-08-20-billing-tied-to-approval` | `assumption-2026-08-20-budget-owner-notification-preference` |
| `decision-2026-08-20-restrict-invite-to-full-members` | `insight-2026-08-18-support-escalation-origin` | — |
| `assumption-2026-08-20-budget-owner-notification-preference` | `risk-2026-08-20-losing-budget-checkpoint`, `assumption-2026-08-20-approval-rarely-blocks` | — |

## Findings (most severe first)

### 1. `blocking` — high-leverage-assumption

**Records:** `assumption-2026-08-20-budget-owner-notification-preference`,
`risk-2026-08-20-losing-budget-checkpoint`

**Reason:** `assumption-2026-08-20-budget-owner-notification-preference`
is the only record in the store addressing
`risk-2026-08-20-losing-budget-checkpoint`, yet it's unvalidated,
low-confidence, and based on the requester's own guess rather than input
from an actual budget owner — if it's wrong, the risk it exists to close
reopens with nothing else standing in for it.

### 2. `worth-addressing` — high-leverage-assumption

**Records:** `assumption-2026-08-20-approval-rarely-blocks`,
`insight-2026-08-20-billing-tied-to-approval`,
`assumption-2026-08-20-budget-owner-notification-preference`

**Reason:** `assumption-2026-08-20-approval-rarely-blocks` is unvalidated
and low-confidence, and is referenced by two other records — enough
leverage to be worth checking against real rejection/modification data —
but unlike Finding 1, nothing in the store depends on it exclusively, so
it's not blocking.

### 3. `worth-addressing` — contradiction

**Records:** `assumption-2026-08-20-approval-rarely-blocks`,
`insight-2026-08-20-billing-tied-to-approval`

**Reason:** `assumption-2026-08-20-approval-rarely-blocks` frames admin
approval as doing "not really... much," while
`insight-2026-08-20-billing-tied-to-approval` documents a concrete function
approval currently serves — the account's only current budget-visibility
checkpoint — so read together, the assumption understates what removing
approval would actually cost.

## Checks that ran and found nothing

**Check B (decision alternatives):**
`decision-2026-08-20-restrict-invite-to-full-members` is the only
`decision` in the store. It has two real alternatives, and the non-chosen
one carries a `rejected_because` — the check clears; this isn't a
foregone conclusion dressed up as a decision.

**Check C (unacknowledged risk):**
`risk-2026-08-20-losing-budget-checkpoint` is the only `risk` in the store,
and it's linked from `assumption-2026-08-20-budget-owner-notification-preference`
via `related_ids` — it has been acknowledged and has a next step attached,
even a weak one. The weakness of that next step is exactly what Finding 1
is about; this check only asks whether a risk was acknowledged at all.

No `minor`-severity findings applied to this store — every gap found had
enough leverage to clear the bar for `worth-addressing` or higher; a
minor finding wasn't manufactured to demonstrate the severity level.
