# Example: strategy-agent — options and recommendation

Continues from `input.md`. Four options for how the budget owner learns
about new self-invited seats, each with a fully-populated `impact`
object. Every specific claim below either names the record id it traces
to, or is prefixed `(strategy-agent's judgment, not evidence)` — the same
standard `research-agent` holds itself to for competitor claims. All JSON
validates against `.claude/memory/schema.json`.

## Options considered

1. **Real-time per-invite notification** — follows TeamFlo
   (`research-source-2026-08-25-teamflo-notification`). Rejected: goes
   beyond what `assumption-2026-08-20-budget-owner-notification-preference`
   says is actually needed.
2. **No dedicated notification; rely on the existing invoice** — echoes
   Huddleworks' outcome (`research-source-2026-08-25-huddleworks-flat-billing`),
   but for a reason that doesn't transfer: Huddleworks avoids the problem
   via flat pricing, and we're per-seat billed
   (`insight-2026-08-20-billing-tied-to-approval`). Rejected: leaves
   `risk-2026-08-20-losing-budget-checkpoint` effectively unaddressed.
3. **Weekly digest** — grounded in our own
   `assumption-2026-08-20-budget-owner-notification-preference`, not
   modeled on any single competitor. **Recommended.**
4. **Persistent ambient seat-count and cost banner** — follows RosterPilot
   (`research-source-2026-08-26-rosterpilot-ambient-banner`). Rejected:
   this is the case that **explicitly argues against following a
   competitor's approach** — the stated preference describes a push
   expectation ("finding out"), and a passive banner risks under-delivering
   on that if the budget owner never looks.

Option 1 is the case that **explicitly follows a competitor's approach**
(TeamFlo). Between them, options 1 and 4 satisfy the requirement to both
engage with and, in option 4's case, deliberately diverge from research
already in the store — not ignore it.

## The decision record

```json
{
  "id": "decision-2026-09-02-budget-owner-notification-mechanism",
  "type": "decision",
  "title": "Notify the budget owner via a weekly digest, not real-time alerts, an ambient banner, or no dedicated notification",
  "summary": "Recommends a weekly digest summarizing new self-invited seats to the budget owner, over real-time per-invite alerts, a persistent ambient banner, or relying on the existing invoice alone.",
  "status": "active",
  "confidence": "medium",
  "data": {
    "alternatives_considered": [
      {
        "option": "Real-time per-invite notification to the budget owner",
        "pros": [
          "Closest option to fully closing risk-2026-08-20-losing-budget-checkpoint, since visibility is continuous rather than periodic",
          "Follows TeamFlo's approach (research-source-2026-08-25-teamflo-notification)"
        ],
        "cons": [
          "Goes beyond what assumption-2026-08-20-budget-owner-notification-preference says is actually needed — that assumption explicitly states real-time is not required",
          "(strategy-agent's judgment, not evidence) Reintroduces per-event interruption in a different form, working against the original idea's goal of removing invite friction"
        ],
        "rejected_because": "assumption-2026-08-20-budget-owner-notification-preference already establishes the requester doesn't need real-time visibility; matching TeamFlo here would over-build relative to the one piece of stated preference we have on this question.",
        "impact": {
          "customer_impact": "Budget owner gets immediate awareness of every new seat — more visibility than assumption-2026-08-20-budget-owner-notification-preference says is needed.",
          "business_impact": "Most directly closes risk-2026-08-20-losing-budget-checkpoint of the four options, since there's no gap between a seat being added and the budget owner knowing.",
          "engineering_complexity": "high",
          "operational_complexity": "medium",
          "scalability_risk": "(strategy-agent's judgment, not evidence) Notification volume scales linearly with invite frequency; teams that add seats often would get frequent interruptions — the kind of pattern research-source-2026-08-26-rosterpilot-ambient-banner suggests larger, faster-growing teams find fatiguing.",
          "long_term_maintainability": "(strategy-agent's judgment, not evidence) Requires maintaining a real-time event/notification pipeline indefinitely for a need that assumption-2026-08-20-budget-owner-notification-preference says is low-urgency, adding ongoing maintenance surface without a demonstrated matching benefit."
        }
      },
      {
        "option": "No dedicated notification; rely on the existing monthly invoice",
        "pros": [
          "(strategy-agent's judgment, not evidence) Zero engineering cost — no new mechanism to build or maintain",
          "Matches Huddleworks' outcome of not building dedicated per-seat notification (research-source-2026-08-25-huddleworks-flat-billing)"
        ],
        "cons": [
          "Unlike Huddleworks, our pricing is per-seat (insight-2026-08-20-billing-tied-to-approval), so a monthly invoice is a much noisier, later signal than their flat-cap model produces for them — a new seat could go unnoticed for weeks",
          "Leaves risk-2026-08-20-losing-budget-checkpoint effectively unaddressed, just delayed to the next billing cycle"
        ],
        "rejected_because": "Huddleworks avoids per-seat notification because their pricing is flat and capped, so the notification problem barely exists for them. We don't have that pricing model — insight-2026-08-20-billing-tied-to-approval confirms new seats add directly to our invoice — so copying their lack of a mechanism would leave risk-2026-08-20-losing-budget-checkpoint open rather than actually mirroring their outcome. (strategy-agent's judgment, not evidence: doing nothing here reads as declining to address the risk, not as a considered tradeoff.)",
        "impact": {
          "customer_impact": "(strategy-agent's judgment, not evidence) Budget owner has no dedicated signal; first learns of new seats when the invoice arrives, potentially weeks later.",
          "business_impact": "Does not meaningfully address risk-2026-08-20-losing-budget-checkpoint — this is close to the status quo the risk was raised against.",
          "engineering_complexity": "low",
          "operational_complexity": "low",
          "scalability_risk": "(strategy-agent's judgment, not evidence) Low engineering risk, but the budget exposure this option was supposed to weigh against scales unaddressed as teams add more seats.",
          "long_term_maintainability": "(strategy-agent's judgment, not evidence) Nothing to build or maintain, but for a reason that undercuts why this needs to be considered a strategy option at all."
        }
      },
      {
        "option": "Weekly digest to the budget owner listing seats added that week",
        "pros": [
          "Matches assumption-2026-08-20-budget-owner-notification-preference's stated preference closely — that assumption's own data.statement names 'weekly or monthly' as sufficient",
          "Addresses risk-2026-08-20-losing-budget-checkpoint with a bounded, predictable engineering surface — one scheduled job, not a real-time event pipeline or a live UI surface"
        ],
        "cons": [
          "(strategy-agent's judgment, not evidence) Up to a week could pass between a seat being added and the budget owner finding out, which would be worse than the real-time option if urgency turns out to matter more than currently believed",
          "Depends on assumption-2026-08-20-budget-owner-notification-preference actually being correct — that assumption is itself unvalidated (flagged as a blocking gap in review-agent's example findings)"
        ],
        "impact": {
          "customer_impact": "Budget owner receives a weekly summary of new seats rather than per-event alerts or an always-on banner — matches what assumption-2026-08-20-budget-owner-notification-preference says is sufficient.",
          "business_impact": "Directly addresses risk-2026-08-20-losing-budget-checkpoint at a cadence the requester already indicated is acceptable, without the real-time option's engineering cost.",
          "engineering_complexity": "low",
          "operational_complexity": "low",
          "scalability_risk": "(strategy-agent's judgment, not evidence) A weekly batch job scales independently of invite frequency — cost stays flat whether a team adds one seat or fifty in a week.",
          "long_term_maintainability": "(strategy-agent's judgment, not evidence) A single scheduled digest job is simpler to maintain than a per-event notification pipeline or an always-current ambient banner that has to stay in sync with live seat counts."
        }
      },
      {
        "option": "Persistent ambient seat-count and projected-cost banner",
        "pros": [
          "(strategy-agent's judgment, not evidence) Always current, no risk of a stale digest between updates",
          "Follows RosterPilot's approach (research-source-2026-08-26-rosterpilot-ambient-banner), which appears aimed at avoiding notification fatigue for teams that add seats frequently"
        ],
        "cons": [
          "(strategy-agent's judgment, not evidence) Passive — depends on the budget owner opening the product and looking, unlike a digest that arrives on its own",
          "(strategy-agent's judgment, not evidence) Requires building and maintaining a live-updating UI surface, a larger and more open-ended engineering commitment than a scheduled digest job"
        ],
        "rejected_because": "RosterPilot's ambient banner is a reasonable model, but assumption-2026-08-20-budget-owner-notification-preference describes a push-style expectation ('finding out') rather than a pull-style one (checking a banner) — a passive banner risks silently under-delivering on the 'they should know' half of that preference if the budget owner simply doesn't look. A pushed digest (the recommended option) more directly matches what was actually asked for.",
        "impact": {
          "customer_impact": "(strategy-agent's judgment, not evidence) Budget owner can check current seat count and projected cost at any time, but isn't proactively told about individual additions.",
          "business_impact": "Addresses risk-2026-08-20-losing-budget-checkpoint only if the budget owner actively checks the banner — passive by design.",
          "engineering_complexity": "medium",
          "operational_complexity": "medium",
          "scalability_risk": "Low, per research-source-2026-08-26-rosterpilot-ambient-banner's own rationale — an always-current banner avoids the notification-fatigue problem a per-event approach would have at scale.",
          "long_term_maintainability": "(strategy-agent's judgment, not evidence) A live UI surface has more ongoing surface area to keep in sync with actual seat/billing state than a scheduled digest job does."
        }
      }
    ],
    "recommendation": "Notify the budget owner via a weekly digest summarizing seats added that week, rather than a real-time per-invite alert, relying on the existing invoice alone, or a persistent ambient banner.",
    "rationale": "This is the only option that directly matches the low-urgency, after-the-fact preference already logged in assumption-2026-08-20-budget-owner-notification-preference (data.statement: 'finding out well after the fact — e.g. weekly or monthly — is sufficient'), while still giving risk-2026-08-20-losing-budget-checkpoint a concrete, bounded mechanism rather than leaving it open (rejected: no dedicated notification) or over-building relative to stated need (rejected: real-time). It deliberately diverges from RosterPilot's ambient-banner approach (rejected; research-source-2026-08-26-rosterpilot-ambient-banner) because that preference describes a push expectation, not a pull one. This recommendation is provisional: it rests on assumption-2026-08-20-budget-owner-notification-preference, which is itself unvalidated — review-agent's example findings already flagged this exact assumption as the store's single blocking gap. Validating it with an actual budget owner remains the highest-leverage next step before implementation.",
    "open_questions": [
      "assumption-2026-08-20-budget-owner-notification-preference has not been validated with an actual budget owner — this recommendation should be revisited if validation contradicts it.",
      "Digest delivery channel (email vs. in-app) is not yet decided.",
      "Whether admin approval is removed at all remains formally undecided in memory (see the Capability Matrix's other 'Not yet decided' row) — this decision addresses notification mechanism only, on the assumption approval removal proceeds; it does not re-decide that, and re-deciding role scope (decision-2026-08-20-restrict-invite-to-full-members) is explicitly out of scope for this recommendation."
    ]
  },
  "tags": ["strategy-recommendation", "self-serve-invites", "billing"],
  "sources": [],
  "related_ids": [
    "risk-2026-08-20-losing-budget-checkpoint",
    "assumption-2026-08-20-budget-owner-notification-preference",
    "research-source-2026-08-25-teamflo-notification",
    "research-source-2026-08-25-huddleworks-flat-billing",
    "research-source-2026-08-26-rosterpilot-ambient-banner",
    "risk-2026-09-02-digest-misses-burst-spend"
  ],
  "owner": "strategy-agent",
  "created_at": "2026-09-02T11:00:00Z",
  "updated_at": "2026-09-02T11:00:00Z"
}
```

## The new risk this comparison surfaced

Evaluating the options exposed a gap specific to the recommended
mechanism itself — not a re-statement of the original risk — so per Step
4 of `.claude/agents/strategy-agent.md`, one `risk` record is written
alongside the decision:

```json
{
  "id": "risk-2026-09-02-digest-misses-burst-spend",
  "type": "risk",
  "title": "A weekly digest could miss a burst of seat additions before the budget owner sees them",
  "summary": "If many self-invited seats are added in a short window, a weekly digest cadence means the budget owner could learn about the resulting cost increase up to a week after it happened — well after it's actionable in the moment.",
  "status": "draft",
  "confidence": "medium",
  "tags": ["self-serve-invites", "billing", "strategy-recommendation"],
  "sources": [],
  "related_ids": ["decision-2026-09-02-budget-owner-notification-mechanism", "risk-2026-08-20-losing-budget-checkpoint"],
  "owner": "strategy-agent",
  "created_at": "2026-09-02T11:05:00Z",
  "updated_at": "2026-09-02T11:05:00Z"
}
```

## What's deliberately absent

No detailed technical or UX design of the weekly digest (delivery
mechanism, email template, scheduling infrastructure) — that's
`solution-design`'s job, not built yet. No re-litigating
`decision-2026-08-20-restrict-invite-to-full-members` or whether admin
approval should be removed at all — both stay exactly as
`discovery-interview` and prior examples left them; this invocation only
ever targeted the one open question about budget-owner notification.
