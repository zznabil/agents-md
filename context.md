# Project context

## Active scope

Sharpen the canonical `AGENTS.md` guidance so agent work better satisfies
human intent without avoidable frustration. `AGENTS.md` remains the only
governance source; this file records the project vocabulary used while
maintaining it.

## Glossary

- **Intent fidelity** — the degree to which an agent's outcome captures both
  the explicit request and the relevant underlying vision.
- **Right-sized completion** — work scoped broadly enough to satisfy the
  intended outcome, but not so broadly that it introduces unwanted changes.
- **System integrity** — preserving relevant behavior and contracts outside
  the requested change, unless the user explicitly authorizes their change.
- **Autonomous adjacent correction** — a change outside the explicit request
  that an agent may make only when it is reversible, tightly coupled to the
  requested work, and required for correctness.
- **Decision-quality consultation** — obtaining targeted advice from an
  available advisor, planner, researcher, or other specialist before a
  consequential decision; consultation informs the accountable agent rather
  than transferring responsibility for the outcome.
- **Required consultation** — decision-quality consultation required before
  an irreversible or high-blast-radius decision, or one that is
  security/data-sensitive, materially ambiguous, or dependent on unfamiliar
  or uncertain facts. It remains optional when it would materially improve
  confidence for lower-risk work.
- **Decision-relevant handoff** — a completion message that starts with the
  outcome; states material assumptions or decisions, verification actually
  performed, affected areas, and any remaining risk or user choice; omits
  routine tool narration and implementation trivia that do not affect a
  decision; and ends with a compact TL;DR.
- **Proportionate discovery** — begin every task with a cheap local-context
  and impact check; expand into references, tests, documentation, or
  specialist consultation only when work crosses a boundary, carries
  meaningful risk, or leaves material uncertainty; stop when the evidence is
  sufficient for a safe, reversible decision.
- **Proportionate pre-mortem** — before shared, architectural, risky, or
  irreversible work, and whenever confidence is low, identify how the work
  could fail the user through intent mismatch, regression or blast radius,
  scope creep, or verification gaps, then mitigate the material risks. Apply
  the same check mentally for tiny or local work without adding ceremony.
- **Calibrated assumption** — an inference an agent may make and act on when
  it is low-risk, reversible, and consistent with surrounding context. State
  material assumptions; ask before proceeding when an assumption would change
  product behavior, scope, cost, security, architecture, or lead credible
  interpretations to materially different outcomes.
- **Considerate agency** — proactive, context-aware follow-through that
  handles ordinary reasoning, discovery, and verification without making the
  human supervise routine decisions, while preserving the human's authority
  over consequential choices.
- **Resilient escalation** — when blocked or verification fails, first seek
  targeted help from available advisors, planners, researchers, or other
  specialists; then exhaust safe, in-scope alternatives and diagnose the
  failure. Return to the human only when new authority, a materially different
  product choice, or an external change is required, with evidence, options, a
  recommendation, and the smallest decision needed.
- **Proportionate quality pass** — for user-visible or shared work, check
  consistency with local conventions, edge and error cases, clarity of names,
  messages, and documentation, accessibility or usability where relevant, and
  avoidable follow-up work. Keep the pass quick for small changes and deeper
  for shared or user-facing work.
- **Conventional default** — for an unspecified subjective choice, follow the
  strongest local convention; otherwise choose the simplest clear,
  accessible, maintainable option. Present alternatives only when they would
  materially change the experience or available context cannot support a
  defensible choice.
- **Considerate Agency** — the named operating model that consolidates this
  repository's expectations for proactive, right-sized, context-aware agent
  work into one dedicated section of the canonical `AGENTS.md`.
- **Considerate Agency section design** — place the section immediately
  before **Final Work Summary**, with the compact subsections **Aim**, **Read
  the Intent**, **Act at the Right Scope**, **Challenge the Work**, and
  **Finish for the Human**.
