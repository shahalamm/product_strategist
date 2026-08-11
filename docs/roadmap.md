# Build order

The platform is built one skill/agent at a time, narrow before wide, per
`CLAUDE.md`'s philosophy. This is a build-order list, not a re-plan — see
`CLAUDE.md` for the project's actual philosophy and principles, and
`CONTRIBUTING.md` for how each of these is required to be shaped.

## Built

1. **`discovery-interview`** (skill) — interrogates a rough,
   underspecified product idea one question at a time, probing vague
   answers before accepting them, and writes `assumption`/`decision`/
   `insight`/`risk` records to the project's memory store.
2. **`review-agent`** (agent) — reads a project's memory store fresh and
   surfaces specific, severity-ranked findings on whether it's defensible
   enough to move toward research or strategy (high-leverage unvalidated
   assumptions, foregone-conclusion decisions, unacknowledged risks,
   contradictions). Reports only — never edits, never gates.
3. **`research-agent`** (agent) — given a project's already-scoped memory
   store, researches competitors and market context specific to that
   scope, writes `research-source`/`competitor` records, and renders a
   Capability Matrix comparing the project's own stance to what was
   found. Refuses to research a broad category if scope isn't established
   yet. May optionally score competitors on 1-3 project-specific axes for
   a future perceptual map, only when a score genuinely traces to
   evidence already gathered. Never positions the project's own stance —
   see `own-position-agent` below.
4. **`strategy-agent`** (agent) — given an already-grounded open
   risk/assumption, generates 2-4 solution options with a structured
   impact assessment each, engages with any existing competitor research
   rather than ignoring it, and recommends exactly one via a single
   tagged `decision` record with a traceable rationale. Doesn't research
   or design — reads memory, writes one decision.
5. **`solution-design`** (skill) — given a specific active `decision` id
   (not only strategy recommendations — a discovery-interview scope
   decision qualifies too), elaborates it into concrete, testable
   `requirement` records. Never re-litigates the target decision and never
   writes a new `decision` for small implementation choices — those get
   resolved inline in a requirement's `details` instead. No output
   document; the requirement records are the deliverable.
6. **`systems-impact`** (agent) — given a decision's buildable
   `requirement` records, assesses what downstream systems and workflows
   (data model, monitoring, customer support, etc.) they concretely
   touch, each finding traceable to a specific requirement with a
   severity. Stops and asks for `solution-design` to run first if the
   target decision has no requirements yet. Identifies impact only —
   never proposes a mitigation, a fix, or a new requirement/decision.
7. **`documentation-agent`** (agent) — takes no target id; reads a
   project's *entire* memory store and renders it against a fixed
   35-section canonical Product Decision Document structure. Writes zero
   memory records — the one agent whose only output is a document.
   Sections the store can't support with real records say so plainly
   ("Not available — no `X` records exist yet") rather than being
   omitted or padded; a final Document Coverage table reports
   populated/partial/not-available across all 35, which is the actual
   test of whether the other six agents' records compose into something
   coherent.
8. **`own-position-agent`** (agent) — finds every perceptual-map axis
   `research-agent` has already scored competitors on, and reports where
   the project's own stance falls on each — reusing the exact axis
   labels, never inventing a new axis, and omitting an axis entirely
   rather than guessing when nothing in the store resolves it. Marks a
   position `status: "draft"` / `confidence: "low"` when the decision it
   derives from is itself provisional, naming the specific unvalidated
   assumption rather than hedging vaguely. `competitor` records stay
   scoped to external products; this is the only agent that positions
   "us."
9. **`interactive-map-agent`** (agent) — renders every axis-pair with at
   least two plottable entities (competitor scores and/or an
   own-position) as an interactive Chart.js scatter map — one
   self-contained HTML file, no build step, opens directly in a browser.
   An entity missing one axis of a pair is simply absent from that map,
   named explicitly in a "not plotted" note rather than forced on with a
   guess. "Us" renders as a hollow marker whenever its position is
   `status: "draft"`. Writes zero memory records — the second agent
   (after `documentation-agent`) whose only output is a document. No
   radar/spider view yet — that needs 3+ consistently-scored axes across
   the same entities, which no project has produced.

## Still to come

Nine pieces are now built: the original seven-agent vision plus the
perceptual-map extension (Tasks 11-13) — scoring, own-position, and now
an actual rendering surface for both. The core discovery → research →
strategy → solution-design → systems-impact → documentation loop is
complete end to end, and competitors/own-position can be plotted and
explored interactively for any project with scored axis data.

Known gaps, not silently omitted: a radar/spider chart (needs 3+ shared
axes, doesn't exist in any project's data yet), and the parked UI track —
a way to interact with this platform outside a Claude Code conversation —
noted here as a pointer to future work, not started or designed in this
repository.
