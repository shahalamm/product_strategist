# Example: review-agent — input

This example reuses, verbatim, the six-record memory store produced by the
`discovery-interview` skill's own worked example (see
`.claude/examples/discovery-interview/`), rather than inventing a store for
the occasion — so this example is testable against something real.

Project slug: `self-serve-team-invites`. The Review Agent is invoked with
this store: `.claude/memory/self-serve-team-invites.json`.

```json
{
  "version": "1.0.0",
  "records": [
    {
      "id": "insight-2026-08-18-support-escalation-origin",
      "type": "insight",
      "title": "Idea originated from a support escalation",
      "summary": "The idea to let team members self-invite others without admin approval came from a support ticket where the only admin was on vacation and couldn't approve new teammates for 3 days, blocking their onboarding.",
      "status": "active",
      "confidence": "medium",
      "tags": ["source-context"],
      "sources": [
        { "label": "Support escalation #4821 - illustrative example", "reference": "n/a - placeholder example, not a real source" }
      ],
      "related_ids": [],
      "owner": "example",
      "created_at": "2026-08-18T14:02:00Z",
      "updated_at": "2026-08-18T14:02:00Z"
    },
    {
      "id": "assumption-2026-08-20-approval-rarely-blocks",
      "type": "assumption",
      "title": "Admin approval rarely rejects or changes an invite",
      "summary": "The requester believes admin approval almost never actually rejects or modifies an invite, but has no data confirming this.",
      "status": "draft",
      "confidence": "low",
      "data": {
        "statement": "In the last month, admin approval has not rejected or modified any team invite.",
        "validation_status": "unvalidated",
        "impact_if_wrong": "If rejections/modifications are actually common, removing approval would silently strip a control admins currently rely on, letting unwanted invites or role errors through unchecked."
      },
      "tags": ["self-serve-invites", "approval-flow"],
      "sources": [],
      "related_ids": ["insight-2026-08-18-support-escalation-origin"],
      "owner": "discovery-interview",
      "created_at": "2026-08-20T09:14:00Z",
      "updated_at": "2026-08-20T09:14:00Z"
    },
    {
      "id": "insight-2026-08-20-billing-tied-to-approval",
      "type": "insight",
      "title": "Approval step is also today's billing visibility checkpoint",
      "summary": "New seats add to the account's invoice, so the current approval step is where someone with budget visibility currently sees a new seat being added, whether or not that's its primary purpose.",
      "status": "active",
      "confidence": "medium",
      "tags": ["self-serve-invites", "billing"],
      "sources": [],
      "related_ids": ["assumption-2026-08-20-approval-rarely-blocks"],
      "owner": "discovery-interview",
      "created_at": "2026-08-20T09:15:00Z",
      "updated_at": "2026-08-20T09:15:00Z"
    },
    {
      "id": "risk-2026-08-20-losing-budget-checkpoint",
      "type": "risk",
      "title": "Removing approval removes the only current budget checkpoint",
      "summary": "If admin approval is removed with nothing in its place, there would be no checkpoint left where someone with budget visibility sees a new seat being added before it hits the invoice.",
      "status": "draft",
      "confidence": "medium",
      "tags": ["self-serve-invites", "billing"],
      "sources": [],
      "related_ids": ["insight-2026-08-20-billing-tied-to-approval"],
      "owner": "discovery-interview",
      "created_at": "2026-08-20T09:16:00Z",
      "updated_at": "2026-08-20T09:16:00Z"
    },
    {
      "id": "decision-2026-08-20-restrict-invite-to-full-members",
      "type": "decision",
      "title": "Restrict self-invite ability to the full-member role",
      "summary": "Self-invite is scoped to full members only; guests and contractors cannot invite new teammates.",
      "status": "active",
      "confidence": "high",
      "data": {
        "alternatives_considered": [
          {
            "option": "Allow every role (including guests and contractors) to invite",
            "cons": ["Widens the risk surface beyond what's been examined", "Contractors/guests may not be accountable for who they add"],
            "rejected_because": "Requester explicitly excluded these roles when asked directly."
          },
          {
            "option": "Allow only the full-member role to invite",
            "pros": ["Matches requester's stated intent", "Keeps the risk surface to accountable, permanent teammates"]
          }
        ],
        "recommendation": "Self-invite is available to full members only; guests and contractors are excluded.",
        "rationale": "Requester confirmed directly that 'any team member' was never meant to include guests or contractors — narrowing scope to full members before any further design work avoids designing against a broader, unintended blast radius.",
        "open_questions": [
          "Are there full-member sub-roles (e.g. read-only) that should also be excluded? Not yet asked."
        ]
      },
      "tags": ["self-serve-invites", "scope"],
      "sources": [],
      "related_ids": ["insight-2026-08-18-support-escalation-origin"],
      "owner": "discovery-interview",
      "created_at": "2026-08-20T09:22:00Z",
      "updated_at": "2026-08-20T09:22:00Z"
    },
    {
      "id": "assumption-2026-08-20-budget-owner-notification-preference",
      "type": "assumption",
      "title": "Budget owner needs to know about new seats, but not urgently",
      "summary": "Requester believes the budget owner needs some visibility into new self-invited seats, but that low-urgency, after-the-fact awareness is sufficient — this hasn't been checked with an actual budget owner, and no mechanism has been decided.",
      "status": "draft",
      "confidence": "low",
      "data": {
        "statement": "The budget owner needs to know when new seats are added via self-invite, but does not need that information in real time — finding out well after the fact (e.g. weekly or monthly) is sufficient.",
        "validation_status": "unvalidated",
        "impact_if_wrong": "If budget owners actually need near-real-time visibility to catch unexpected spend before it compounds, a low-urgency approach would let them find out too late to act — undermining the risk this assumption was meant to address."
      },
      "tags": ["self-serve-invites", "billing"],
      "sources": [],
      "related_ids": ["risk-2026-08-20-losing-budget-checkpoint", "assumption-2026-08-20-approval-rarely-blocks"],
      "owner": "discovery-interview",
      "created_at": "2026-08-20T09:31:00Z",
      "updated_at": "2026-08-20T09:31:00Z"
    }
  ]
}
```
