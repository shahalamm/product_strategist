# Example: documentation-agent — rendering notes

Continues from `input.md`. The full rendered document is
`product-decision-document.example.md` — this file highlights the parts
worth calling out, especially the honesty-rule demonstration in Section
27, rather than repeating the whole thing.

## What's populated, partial, and not available

Of the 35 canonical sections, 21 are `Populated`, 1 is `Partial`, and 13
are `Not available` — see the Document Coverage table at the end of the
rendered document for the full breakdown with reasons. Two things worth
noting about that split:

- **The `Partial` section (22, Edge Cases)** exists because 3 of the 4
  `requirement` records specify `edge_cases` and one
  (`requirement-2026-09-05-digest-running-total-and-cost`) doesn't. That's
  read directly off the records — `documentation-agent` didn't invent an
  edge case to fill the gap, and didn't silently present the section as
  fully populated either.
- **13 sections are honestly empty**, not padded. `Personas`,
  `Market Signals`, `Opportunities`, and `Feature Ideas` are empty because
  no skill in this platform has ever produced those record types — that's
  the deliberate design point of this example (see `input.md`). The other
  9 (`Goals & Success Metrics`, `Non-Goals`, `Constraints`, `Dependencies`,
  `Security & Privacy Considerations`, `Rollout & Launch Plan`, `Roadmap`,
  `Future Enhancements`, `Stakeholders & Approvals`) are empty because
  this schema simply has no record type that maps to them yet.

## Section 27 (Risks & Mitigations): the honesty rule in practice

This is the section the task specifically asked this example to
demonstrate. All three `risk` records get a section, but the schema has
**no `mitigation` record type at all** — so nothing in this platform can
ever produce a record that literally calls itself a mitigation. The
rendering treats that fact three different ways, matched to what the
records actually say:

1. **`risk-2026-08-20-losing-budget-checkpoint`** — something *does*
   address it: `decision-2026-09-02-budget-owner-notification-mechanism`'s
   own rationale frames the weekly-digest recommendation as giving this
   risk "a concrete, bounded mechanism." The document reports that
   framing, attributed to the decision, without upgrading it to
   "Mitigated" — a decision addressing a risk is not the same as a
   verified resolution.

2. **`risk-2026-09-02-digest-misses-burst-spend`** — nothing addresses
   it, and one record says so about itself:
   `requirement-2026-09-05-digest-cadence`'s own `edge_cases` states
   verbatim that "this requirement does not add burst detection or an
   earlier trigger." The document renders that self-disclaimer as-is
   rather than softening it into something that sounds more resolved.

3. **`risk-2026-09-10-silent-digest-failure`** — same pattern:
   `system-impact-2026-09-10-monitoring-silent-failure`'s own `details`
   names the shape of a plausible fix ("a positive 'ran, found nothing'
   heartbeat") while explicitly stating that writing it is out of scope
   for that agent. No record anywhere in the store actually implements
   that fix, so the document says exactly that — the risk is named and
   assessed, not resolved.

Three risks, three different actual states in memory (addressed-by-a-
decision-but-unverified; explicitly-acknowledged-and-unaddressed;
explicitly-acknowledged-and-unaddressed-with-a-named-follow-up-shape) —
`documentation-agent` reports each faithfully rather than collapsing them
into a uniform "risks and mitigations" list that implies more closure
than the store actually contains.

## What's deliberately absent from this example

No new record was written anywhere — `documentation-agent` writes zero
memory records, by design. No mitigation, requirement, or decision was
invented to make Section 27 look more resolved than the store supports.
No section was skipped, even the 13 that render nothing but a "Not
available" line.
