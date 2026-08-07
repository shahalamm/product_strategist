# Example: discovery-interview — "self-invite for teams"

This is a worked example of the `discovery-interview` skill, showing a
realistic rough idea, the memory context that already exists before the
interview starts, and four interview turns. It is illustrative
documentation, not a real session — `owner` is set to `"example"` on every
record, matching the convention used in `.claude/memory/README.md`.

## The rough idea

The user, mid-conversation, says:

> "We should let any team member invite new teammates without needing an
> admin to approve every seat. Admin approval is just friction at this
> point."

This is exactly the kind of input the skill should trigger on: a pitch with
an unstated problem, an unscoped "any team member," and an unexamined
claim ("just friction") baked in as if it were settled.

## Pre-existing memory (before this interview session)

Project slug: `self-serve-team-invites`. Store file:
`.claude/memory/self-serve-team-invites.json`. Before this session, one
record already exists from an earlier conversation — the skill reads this
in Step 0 and must not re-ask where the idea came from:

```json
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
}
```

Because this is already known, the skill does not ask "where did this idea
come from" or "why now" — it starts from the highest remaining uncertainty
instead. See `output.md` for the four turns that follow.
