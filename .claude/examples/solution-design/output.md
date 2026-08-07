# Example: solution-design — requirements

Continues from `input.md`. Four `requirement` records elaborate
`decision-2026-09-02-budget-owner-notification-mechanism` into buildable
specifics. No new `decision` record is written anywhere in this example —
the one small implementation choice it surfaces (delivery channel) is
resolved inline in a requirement's `details`, per Step 4. Every specific
claim in `details`/`edge_cases` either names the record it traces to or is
marked `(solution-design's judgment, not evidence)`. All JSON validates
against `.claude/memory/schema.json`.

## 1. Core digest content and trigger — carries forward the caveat

```json
{
  "id": "requirement-2026-09-05-digest-content-and-trigger",
  "type": "requirement",
  "title": "Weekly digest email lists seats added in the prior 7 days",
  "summary": "The system sends the budget owner a weekly digest email listing every seat added via self-invite in the preceding 7 days, or sends nothing if none were added.",
  "status": "active",
  "confidence": "high",
  "data": {
    "statement": "The system sends a weekly digest email to the budget owner listing every seat added via self-invite in the preceding 7 days.",
    "acceptance_criteria": [
      "If at least one seat was added via self-invite in the preceding 7 days, exactly one digest email is sent to the budget owner on the scheduled digest day.",
      "If zero seats were added via self-invite in the preceding 7 days, no digest email is sent for that period.",
      "Each seat listed in the digest includes the date it was added and the name or email of whoever added it, if available."
    ],
    "edge_cases": [
      "(solution-design's judgment, not evidence) If more seats are added between when the digest is generated and when it's actually delivered (e.g. a send failure and retry), the digest covers seats added up through generation time, not delivery time, to avoid double-counting a seat on retry."
    ],
    "elaborates_decision_id": "decision-2026-09-02-budget-owner-notification-mechanism"
  },
  "details": "Implements the recommendation in decision-2026-09-02-budget-owner-notification-mechanism, which itself rests on assumption-2026-08-20-budget-owner-notification-preference — an unvalidated assumption about what cadence the budget owner actually needs. If that assumption is later invalidated, this requirement (and the weekly-digest mechanism generally) should be revisited rather than treated as settled. The choice to send nothing when there's nothing to report is (solution-design's judgment, not evidence): an empty digest would be noise the low-urgency preference in assumption-2026-08-20-budget-owner-notification-preference doesn't call for.",
  "tags": ["self-serve-invites", "billing"],
  "sources": [],
  "related_ids": ["decision-2026-09-02-budget-owner-notification-mechanism", "assumption-2026-08-20-budget-owner-notification-preference"],
  "owner": "solution-design",
  "created_at": "2026-09-05T09:00:00Z",
  "updated_at": "2026-09-05T09:00:00Z"
}
```

## 2. Delivery channel — the small implementation choice, resolved inline

```json
{
  "id": "requirement-2026-09-05-digest-delivery-channel",
  "type": "requirement",
  "title": "Digest is delivered by email, not an in-app notification",
  "summary": "The weekly digest is delivered to the budget owner via email rather than an in-app notification or persistent banner.",
  "status": "active",
  "confidence": "medium",
  "data": {
    "statement": "The weekly digest is delivered to the budget owner via email, not an in-app notification or banner.",
    "acceptance_criteria": [
      "The digest arrives in the email inbox associated with the budget owner's account, not solely as an in-app notification.",
      "No equivalent digest content is duplicated as a persistent in-app banner or notification badge."
    ],
    "edge_cases": [
      "(solution-design's judgment, not evidence) If the budget owner has no verified email address on file, the digest fails loudly (e.g. flags the account as missing a required notification target) rather than silently never sending — silent failure would recreate the exact visibility gap risk-2026-08-20-losing-budget-checkpoint was raised about."
    ],
    "elaborates_decision_id": "decision-2026-09-02-budget-owner-notification-mechanism"
  },
  "details": "decision-2026-09-02-budget-owner-notification-mechanism's own open_questions left the delivery channel (email vs. in-app) undecided. This is a small implementation choice, not a meaningfully-weighed strategic alternative, so it's resolved here rather than in a new decision record: email is chosen because it doesn't require the budget owner to be actively using the product to receive it, which matches the passive, after-the-fact nature already established by assumption-2026-08-20-budget-owner-notification-preference. (solution-design's judgment, not evidence): an in-app-only notification risks being missed by a budget owner who isn't a frequent user of the self-invite flow itself.",
  "tags": ["self-serve-invites", "billing"],
  "sources": [],
  "related_ids": ["decision-2026-09-02-budget-owner-notification-mechanism", "assumption-2026-08-20-budget-owner-notification-preference", "risk-2026-08-20-losing-budget-checkpoint"],
  "owner": "solution-design",
  "created_at": "2026-09-05T09:05:00Z",
  "updated_at": "2026-09-05T09:05:00Z"
}
```

No `decision` record was written for this choice — the reasoning lives
entirely in this requirement's `details`, per Step 4 of
`.claude/skills/solution-design/SKILL.md`.

## 3. Cadence — the edge case tracing to strategy-agent's surfaced risk

```json
{
  "id": "requirement-2026-09-05-digest-cadence",
  "type": "requirement",
  "title": "Digest cadence is a fixed, non-overlapping 7-day window",
  "summary": "The digest is sent on a consistent day of the week, covering exactly the 7 days since the previous digest, with no gap or overlap.",
  "status": "active",
  "confidence": "high",
  "data": {
    "statement": "The digest cadence is fixed at 7 days, aligned to a consistent day of the week for a given account, not a rolling window recalculated from account creation.",
    "acceptance_criteria": [
      "Digest emails for a given account are always sent on the same day of the week.",
      "The reporting window for each digest covers exactly the 7 days since the previous digest was generated, with no gap and no overlap."
    ],
    "edge_cases": [
      "risk-2026-09-02-digest-misses-burst-spend: if many seats are added in a short burst within a single 7-day window, they all appear together in the same digest rather than triggering any earlier notice. This requirement does not add burst detection or an earlier trigger — that remains the open risk the strategy recommendation accepted, not something this requirement silently resolves."
    ],
    "elaborates_decision_id": "decision-2026-09-02-budget-owner-notification-mechanism"
  },
  "tags": ["self-serve-invites", "billing"],
  "sources": [],
  "related_ids": ["decision-2026-09-02-budget-owner-notification-mechanism", "risk-2026-09-02-digest-misses-burst-spend"],
  "owner": "solution-design",
  "created_at": "2026-09-05T09:10:00Z",
  "updated_at": "2026-09-05T09:10:00Z"
}
```

This is the requirement the task's edge-case requirement targets: the
edge case names `risk-2026-09-02-digest-misses-burst-spend` — the risk
`strategy-agent`'s example surfaced when it recommended the weekly
cadence — directly, rather than silently re-solving or ignoring it.

## 4. Running total and cost context

```json
{
  "id": "requirement-2026-09-05-digest-running-total-and-cost",
  "type": "requirement",
  "title": "Digest includes running seat total and estimated monthly cost",
  "summary": "Each digest includes the account's current total seat count and estimated monthly cost, not just the list of newly added seats.",
  "status": "active",
  "confidence": "medium",
  "data": {
    "statement": "Digest content includes a running total seat count and current estimated monthly cost impact, in addition to the list of newly added seats.",
    "acceptance_criteria": [
      "The digest email displays the total number of seats on the account as of digest generation time.",
      "The digest email displays the estimated monthly cost at the current seat count, using the account's current per-seat rate."
    ],
    "elaborates_decision_id": "decision-2026-09-02-budget-owner-notification-mechanism"
  },
  "details": "(solution-design's judgment, not evidence) decision-2026-09-02-budget-owner-notification-mechanism's rationale focuses on notifying about new seats, but a bare list of names without a running total or cost context would require the budget owner to do their own math to understand impact. Including these fields makes each digest self-contained without requiring the budget owner to cross-reference the invoice separately.",
  "tags": ["self-serve-invites", "billing"],
  "sources": [],
  "related_ids": ["decision-2026-09-02-budget-owner-notification-mechanism"],
  "owner": "solution-design",
  "created_at": "2026-09-05T09:15:00Z",
  "updated_at": "2026-09-05T09:15:00Z"
}
```

## What's deliberately absent

No `decision` record was written in this example — four `requirement`
records, and nothing else. `decision-2026-09-02-budget-owner-notification-mechanism`
itself, `decision-2026-08-20-restrict-invite-to-full-members`, and the
open question of whether admin approval is removed at all are all
untouched: none of them were the target decision, and re-litigating any
of them is explicitly out of scope for this skill.
