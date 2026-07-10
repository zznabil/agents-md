# AGENTS.md — Single Source of Truth

This file is the only governance document for any project under this directory. Do not create parallel policy docs. If a rule changes, update this file and retire the old source.

The purpose of this file is to make AI-assisted work safer, clearer, and easier to maintain without turning development into ceremony.

## 0) Prime Directive

Build durable, understandable software with the smallest safe change that satisfies the user’s intent.

Prioritize, in order:

1. correctness and user intent;
2. safety and reversibility;
3. maintainability and clarity;
4. appropriate velocity;
5. elegance only when it does not harm the above.

Do not let process replace engineering judgment. Rules in this file are guardrails, not an excuse to stop thinking.

## 1) Proportionality and Task Tiers

Apply these rules proportionally to the size, risk, and permanence of the change.

Use the lightest workflow that is still safe.

### Tiny change

Examples: typo fix, comment edit, formatting-only change, small documentation clarification, obvious one-line local fix.

Expected behavior:

- inspect the target file;
- make the direct change;
- run only the checks that are cheap and relevant;
- do not update unrelated docs;
- do not create new documentation artifacts.

### Local change

Examples: one focused file, isolated function, small UI adjustment, targeted bug fix, small test addition.

Expected behavior:

- inspect local context before editing;
- preserve existing style and conventions;
- make the smallest correct change;
- run targeted tests, linting, or type checks when available;
- update docs only if behavior, assumptions, or public expectations changed.

### Shared change

Examples: exported symbols, shared modules, public APIs, reused helpers, cross-file behavior, storage schemas, protocols, integrations.

Expected behavior:

- inspect references and call paths before editing;
- use LSP, codegraph, AST tools, or other structural tools when they materially improve safety;
- update or add tests around the changed behavior;
- update affected living docs when system meaning changes;
- mention blast-radius considerations in the final summary.

### Architectural change

Examples: module boundaries, data model, state machine, event flow, deployment shape, security posture, long-term dependency, major framework choice.

Expected behavior:

- inspect `context.md` and relevant `docs/` artifacts before implementation;
- consider whether an ADR is needed;
- update diagrams, acceptance criteria, state/event docs, or architecture docs as relevant;
- verify that code, tests, and docs tell the same story;
- summarize tradeoffs, risks, and verification.

### Risky or irreversible change

Examples: deletion of important files, data migration, destructive commands, secret handling, dependency replacement, mass rewrite, permission/security changes.

Expected behavior:

- prefer reversible steps;
- preserve or identify rollback paths;
- require explicit user approval before destructive operations outside the requested scope;
- avoid broad rewrites unless the user asked for them or the existing design blocks the task.

If the correct tier is unclear, start slightly safer, then simplify when the extra process adds no value.

## 2) Context Discovery

Before non-trivial work, quickly orient on the project.

Read or inspect, when present and relevant:

- `AGENTS.md`;
- root `context.md`;
- relevant files under `docs/`;
- package/project manifests;
- existing tests and examples;
- nearby code and naming conventions.

Do not assume project structure from memory. Re-ground on the actual file state before editing and after significant edits.

## 3) Architecture Mandate

Projects under this directory default to **functional core / imperative shell**.

This is a strong default, not a religion.

- **Pure functions are the default.** Domain logic should be deterministic, side-effect free, and easy to test.
- **State lives in plain data containers.** Keep mutable state isolated in small, explicit structs/classes/dataclasses/records with little or no business logic.
- **I/O belongs in the imperative shell.** UI, filesystem, network, input devices, timing, threads, persistence, and external services stay at the edges.
- **Dependencies are explicit.** Prefer parameters, interfaces, function references, or adapters over hidden globals.
- **Composition over inheritance.** Prefer function composition and object composition. Avoid inheritance hierarchies for domain behavior.
- **Classes are allowed when useful.** Use classes for lifecycle, adapters, UI shells, resources, framework integration, or boundary objects; keep them thin and boring.

### Framework-native exception

Do not fight the framework.

Framework-native patterns are allowed when they are the simplest maintainable solution, especially in UI frameworks, game loops, ECS systems, ORMs, actor systems, dependency-injection frameworks, plugins, and lifecycle-heavy code.

When using framework-native stateful patterns:

- keep domain rules as pure or easily testable as practical;
- isolate side effects and framework calls;
- make state transitions explicit;
- avoid leaking framework concerns into unrelated domain logic;
- document architectural consequences if the choice is durable.

## 4) Determinism and Reproducibility

Make behavior explicit, deterministic, and reproducible unless a boundary concern requires otherwise.

- Domain logic should be deterministic whenever practical.
- Functions should depend only on explicit inputs.
- Avoid implicit dependencies on time, randomness, environment variables, or global state unless injected at the boundary.
- Isolate nondeterministic operations — clock, UUID generation, randomness, network, filesystem — behind interfaces, adapters, or explicit shell code.
- Prefer reproducible builds, tests, and tooling.
- Preserve deterministic behavior in the functional core during all changes, including AI-assisted edits.

## 5) Strategy and Extensibility

Use the **strategy pattern** for genuinely pluggable behavior.

- New variants should be added by writing a new function/object and registering it in the appropriate registry, dispatch table, or plugin seam.
- Do not hard-code variant selection into the orchestrator when a strategy seam already exists.
- Strategy interfaces must be narrow, explicit, and stable.
- Do not smuggle a generic `config` blob through strategy boundaries. Pass only what the strategy actually needs.
- Keep strategy registration centralized and obvious.

Do not invent a strategy layer for trivial one-off behavior. Prefer simple direct code until variation is real or clearly imminent.

## 6) SOLID, Separation of Concerns, and Modularity

Treat these as practical engineering constraints, not slogans.

- **Single Responsibility:** one module, one job.
- **Open/Closed:** extend by adding a strategy, adapter, or pure helper rather than rewriting the core.
- **Liskov Substitution:** every strategy or implementation must honor the exact contract it advertises.
- **Interface Segregation:** small interfaces, small inputs, small outputs.
- **Dependency Inversion:** high-level code depends on abstractions, interfaces, protocols, or function references rather than concrete implementation details.
- **Separation of Concerns:** keep orchestration, domain logic, persistence, UI, and integration boundaries distinct.
- **High Cohesion:** related behavior stays together.
- **Low Coupling:** modules should know as little as possible about each other.
- **Composability:** prefer small helpers that combine cleanly.
- **Modularity:** build reusable units with sharp boundaries.

## 7) Code Smell Response

Code smells are treated as defects in production-quality code, but respond proportionally.

Common defects:

- Long Method
- God Class
- Feature Envy
- Dead Code
- Magic Numbers
- Shotgun Surgery
- Duplication
- Circular Dependencies
- Technical Debt disguised as "future work"
- Low Cohesion
- Tight Coupling

Rules:

- If a function grows too large, extract smaller pure functions.
- If a module becomes a grab bag, split it.
- If behavior is duplicated, centralize it once.
- If a dependency cycle appears, break it at the seam.
- If a constant matters, name it.
- If code is obsolete, delete it.

When a smell is outside the task scope, do not perform an opportunistic large refactor unless it is necessary for the requested change. Instead, mention the risk or leave a focused TODO only when it adds real value.

## 8) Developer Debugging Experience

Optimize for fast diagnosis.

- Favor deterministic logic and explicit inputs/outputs.
- Keep errors clear, contextual, and actionable.
- Avoid silent failures, swallowed exceptions, and ambiguous fallbacks.
- Use small functions so failures are easy to isolate.
- Keep state transitions observable.
- Prefer narrow, testable seams over clever abstraction.
- Preserve reproducibility: if a bug happens once, make it easy to reproduce again.

## 9) Tooling and Editing Discipline

Use the best available tool for the task, matched to the risk tier.

### Preferred tools

- **codegraph**: use when an index exists and the task affects shared code, architecture, call paths, or blast radius.
- **LSP**: use for navigation, references, rename, diagnostics, and refactors when a language server is available.
- **AST tools** (`ast_grep`, `ast_edit`): use for structural discovery and structural rewrites such as declarations, callsites, signatures, imports, and codemods.
- **Hashline edit**: use for precise, line-aware modifications when editing by line is safest.
- **Plain text search/edit**: acceptable for tiny/local edits, prose, config, or when better tools are unavailable or clearly unnecessary.

### Mandatory habits

- Before renaming an exported symbol, inspect references first.
- Before editing shared or central code, inspect dependencies and call paths.
- After every significant edit, re-ground on the updated file state before continuing.
- Prefer the smallest correct change.
- Do not use broad text replacement for structural code changes when AST, LSP, or other structural tooling is available and appropriate.
- If a preferred tool is unavailable, use the next safest method and be transparent in the final summary when it matters.

Tooling should improve confidence. It should not become performance theatre.

## 10) Testing and Verification

Every meaningful code change needs credible verification.

- Run the narrowest useful test first.
- Prefer targeted tests for local changes and broader tests for shared or architectural changes.
- Run formatters, linters, type checks, or build checks when they are part of the project workflow and relevant to the change.
- Add or update tests when behavior changes or when a bug fix would otherwise be easy to regress.
- Do not claim that tests passed unless they were actually run.
- If tests cannot be run, state why and describe the best available verification performed.

Verification should be proportional. A typo fix does not need a full build unless the project requires it.

### Test-driven development

For behavior changes with a clear, testable contract, use test-driven development by default.

1. Identify the next observable behavior and its public interface.
2. Write one focused test that fails for the expected reason.
3. Implement only enough code to make that test pass.
4. Refactor only while the relevant tests are green.
5. Repeat one behavior at a time, then run the proportionate broader checks.

Tests should describe externally observable behavior and survive internal refactors. Prefer real code paths and narrow fakes at external boundaries; do not couple tests to private methods or incidental implementation details.

Do not write an entire test suite before implementation. Work in vertical slices: one behavior, one failing test, one minimal implementation.

TDD is not required for typo-only changes, documentation-only edits, exploratory spikes, or boundary-heavy work where a meaningful automated test is impractical. In those cases, use the strongest proportionate verification available and state what was checked.

## 11) Implementation Style

- Favor pure functions over imperative code whenever practical.
- Keep imperative orchestration thin, explicit, and boring.
- Use classes only when they add real value at the shell, boundary, lifecycle, framework, or resource layer.
- Keep object methods small and compositional.
- Avoid inheritance except where a framework requires it or a boundary object is genuinely simpler that way.
- Delete dead code, unused shims, and temporary compatibility layers once migration is complete.
- Keep modules focused and boring.
- Avoid cleverness that makes future debugging harder.

## 12) Source-of-Truth Discipline

- This file supersedes any older governance or practices document in this directory.
- Do not introduce competing instructions in another markdown file.
- If a future rule conflicts with this file, update this file rather than layering a second policy doc on top.
- Project docs may describe system facts, decisions, diagrams, and current context, but they must not create policy that competes with `AGENTS.md`.

## 13) Living Engineering Documentation

Project documentation is local, version-controlled, and kept current when it is relevant. These documents are engineering artifacts, not competing governance sources. **AGENTS.md remains the only policy source.**

Documentation must be useful, current, and close to the actual system. Avoid documentation theatre.

### Canonical documentation location

Maintain the active project context at the project root as `context.md`.

`context.md` is allowed at root because agents need to discover it quickly before planning or editing. It is a living project artifact, not a governance source. It must not contain rules that compete with `AGENTS.md`.

Maintain the remaining project documentation under `docs/` unless the project already has a clearly established documentation directory.

Use canonical lowercase filenames and avoid duplicate responsibility:

- `context.md` — root-level current project context, constraints, assumptions, glossary, active scope, and latest working notes needed before planning.
- `docs/user-stories.md` — user goals, roles, stories, and product expectations.
- `docs/acceptance-criteria.md` — testable acceptance criteria, done conditions, and verification checklist.
- `docs/architecture-diagram.md` — high-level architecture, major components, boundaries, and dependencies.
- `docs/block-diagram.md` — block-level system/component decomposition.
- `docs/sequence-diagrams.md` — Mermaid sequence diagrams for important flows.
- `docs/activity-diagrams.md` — activity diagrams for workflows and business processes.
- `docs/flowcharts.md` — procedural decision flows that are clearer as flowcharts.
- `docs/state-machines.md` — state machines, state diagrams, transitions, guards, and side effects.
- `docs/use-case-diagram.md` — actors, use cases, system boundary, and relationships.
- `docs/event-flow-diagram.md` — domain events, producers, consumers, message flow, and ordering assumptions.
- `docs/uml.md` — UML overview or index linking to UML-style diagrams; do not duplicate content already owned by a more specific document.
- `docs/adr/` — Architecture Decision Records, one decision per file.
- `docs/changelog.md` — user-visible or externally meaningful changes.
- `docs/audit.md` — implementation audit trail for meaningful AI-assisted or human-assisted changes.

Not every project must instantiate every document immediately. Create or maintain a document when it has real content and improves future work. Do not create empty placeholder docs unless the project is being initialized and the placeholder has an immediate purpose.

Do not create both root and `docs/` variants of `context.md`. If `docs/context.md` already exists, migrate or redirect its content into root `context.md` and retire the duplicate.

Do not create both uppercase and lowercase variants of the same document. If older files such as `STATE-MACHINES.md`, `SEQUENCE-DIAGRAMS.md`, or `ACTIVITY-DIAGRAM.md` already exist, migrate or redirect their content into the canonical lowercase file and retire the duplicate.

### Documentation update rule

Before and after any non-trivial change, identify which living docs are affected.

Update the relevant docs in the same change when any of these change:

- architecture, boundaries, modules, dependencies, or deployment shape;
- user-visible behavior, product scope, or acceptance criteria;
- state transitions, workflow logic, event flow, orchestration, or business process;
- public APIs, integrations, protocols, storage schemas, or message contracts;
- major implementation strategy, tradeoff, constraint, or decision;
- risks, known limitations, migration notes, or verification steps.

If a change has no documentation impact, leave the docs untouched. Do not perform noisy timestamp-only edits.

### Diagram rules

Prefer Mermaid inside Markdown for diagrams unless the project has a stronger existing convention.

Every diagram document should include, when useful:

- a short purpose statement;
- the scope covered by the diagram;
- the diagram source in editable text form;
- important assumptions and constraints;
- a "last meaningfully updated" note tied to the change, not a mechanical timestamp.

Diagrams must describe the current intended system, not outdated history. If a diagram becomes obsolete, update it or clearly mark it as retired with a replacement link.

Do not maintain diagrams for flows that are too trivial to justify them. Prefer one accurate compact diagram over many stale detailed diagrams.

### ADR rules

Use ADRs for durable decisions that affect architecture, dependencies, data model, interfaces, deployment, security, performance, or long-term maintenance.

Each ADR should include:

- status: proposed, accepted, superseded, or rejected;
- context;
- decision;
- consequences;
- alternatives considered;
- links to related docs, issues, or implementation areas.

Do not create an ADR for trivial implementation details.

### Audit and changelog rules

`docs/audit.md` records meaningful engineering changes and why they were made. Include enough context for future maintainers to understand intent, risk, and verification.

`docs/changelog.md` records externally meaningful changes. Do not turn it into a raw commit log.

For each meaningful change, update audit/changelog only when relevant:

- Audit entry: change summary, files/areas touched, reason, risk, verification performed.
- Changelog entry: added, changed, fixed, removed, deprecated, or security impact.

### Staleness prevention

Documentation must not become decorative.

When editing code, tests, configuration, or project behavior:

1. inspect the relevant existing docs before changing behavior;
2. update affected docs after the implementation;
3. remove obsolete documentation instead of preserving contradictions;
4. ensure docs and implementation tell the same story;
5. mention any documentation updates in the final work summary.

If implementation and docs conflict, treat it as a defect. Resolve the conflict by checking the code, tests, user request, and current project intent before changing either side.

### Anti-bloat guardrails

Keep documentation useful, not bureaucratic.

- Do not create empty placeholder docs unless the project is being initialized and the placeholder is immediately useful.
- Do not duplicate the same diagram across multiple files.
- Do not write project governance rules into `docs/`; governance belongs only in `AGENTS.md`.
- Do not maintain diagrams for flows that are too trivial to justify them.
- Prefer one accurate compact diagram over many stale detailed diagrams.
- Keep docs close to the actual system and prune aggressively.

## 14) Prototype Mode

Prototype work may temporarily favor speed and learning over perfect architecture.

Prototype mode is allowed when the goal is exploration, MVP discovery, throwaway validation, or fast user feedback.

Rules for prototype mode:

- keep shortcuts visible;
- avoid hiding risky assumptions;
- prefer simple working slices over broad infrastructure;
- do not prematurely create every documentation artifact;
- mark cleanup paths before promoting prototype code to production-quality code;
- preserve safety around secrets, data loss, migrations, and external side effects.

A prototype can be messy. It must not be misleading.

## 15) Autonomy and Escalation

Agents may act autonomously for reversible changes that clearly satisfy the user’s request and stay within the current scope.

Escalate or ask for confirmation before:

- destructive actions outside the requested scope;
- deleting user data or important project history;
- changing secrets, credentials, permissions, or security posture;
- replacing major dependencies or frameworks;
- broad rewrites when a targeted change would work;
- making product or architectural decisions with unclear user intent.

When user intent is clear, do not stall with unnecessary questions. Make the best safe change and document assumptions.

## 16) Considerate Agency

Agents should reduce the need for human supervision through considerate,
context-aware judgment. Complete the intended outcome, not merely the literal
task, while preserving the user's authority over consequential choices.

### Aim

- Optimize for intent fidelity, right-sized completion, and system integrity.
- Anticipate ordinary needs, follow through on safe work, and avoid making the
  user manage routine decisions or recover from preventable omissions.

### Read the Intent

- Start with a cheap local-context and impact check; expand discovery only
  when the work crosses a boundary, carries meaningful risk, or leaves
  material uncertainty.
- Infer and act on assumptions that are low-risk, reversible, and consistent
  with surrounding context. State material assumptions.
- Ask before proceeding when an assumption would change product behavior,
  scope, cost, security, or architecture, or when credible interpretations
  lead to materially different outcomes.
- For unspecified subjective choices, follow the strongest local convention;
  otherwise choose the simplest clear, accessible, maintainable option.

### Act at the Right Scope

- Make an adjacent correction autonomously only when it is reversible, tightly
  coupled to the requested work, and required for correctness.
- Consult an available advisor, planner, researcher, or other specialist
  before an irreversible or high-blast-radius decision, or one that is
  security/data-sensitive, materially ambiguous, or based on unfamiliar or
  uncertain facts. Consultation informs the accountable agent; it does not
  transfer responsibility.
- When blocked, seek targeted advice and exhaust safe, in-scope alternatives
  before returning to the user. Escalate only for new authority, a materially
  different product choice, or an external change.

### Challenge the Work

- Before shared, architectural, risky, or irreversible work, and whenever
  confidence is low, run a brief pre-mortem covering intent mismatch,
  regression or blast radius, scope creep, and verification gaps. Apply the
  same check mentally for tiny or local work without adding ceremony.
- For user-visible or shared work, perform a proportionate quality pass:
  check local conventions, edge and error cases, clarity, relevant
  accessibility or usability, and avoidable follow-up work.

### Finish for the Human

- Start every completion with the outcome. For non-trivial work, state the
  material assumptions or decisions, verification actually performed,
  affected areas, and any remaining risk or user choice.
- Omit routine tool narration and implementation trivia unless it changes a
  decision. End with a compact TL;DR.

## 17) Final Work Summary

For non-trivial work, finish with a concise summary covering:

- what changed;
- files or areas touched;
- tests, checks, or verification performed;
- documentation updated or intentionally left unchanged;
- risks, limitations, or follow-up work that matters.

Do not claim certainty beyond the evidence. Be explicit about anything not run, not checked, or intentionally deferred.

<!-- code-review-graph MCP tools -->
## MCP Tools: code-review-graph

**IMPORTANT: This project has a knowledge graph. ALWAYS use the
code-review-graph MCP tools BEFORE using Grep/Glob/Read to explore
the codebase.** The graph is faster, cheaper (fewer tokens), and gives
you structural context (callers, dependents, test coverage) that file
scanning cannot.

### When to use graph tools FIRST

- **Exploring code**: `semantic_search_nodes` or `query_graph` instead of Grep
- **Understanding impact**: `get_impact_radius` instead of manually tracing imports
- **Code review**: `detect_changes` + `get_review_context` instead of reading entire files
- **Finding relationships**: `query_graph` with callers_of/callees_of/imports_of/tests_for
- **Architecture questions**: `get_architecture_overview` + `list_communities`

Fall back to Grep/Glob/Read **only** when the graph doesn't cover what you need.

### Key Tools

| Tool | Use when |
| ------ | ---------- |
| `detect_changes` | Reviewing code changes — gives risk-scored analysis |
| `get_review_context` | Need source snippets for review — token-efficient |
| `get_impact_radius` | Understanding blast radius of a change |
| `get_affected_flows` | Finding which execution paths are impacted |
| `query_graph` | Tracing callers, callees, imports, tests, dependencies |
| `semantic_search_nodes` | Finding functions/classes by name or keyword |
| `get_architecture_overview` | Understanding high-level codebase structure |
| `refactor_tool` | Planning renames, finding dead code |

### Workflow

1. The graph auto-updates on file changes (via hooks).
2. Use `detect_changes` for code review.
3. Use `get_affected_flows` to understand impact.
4. Use `query_graph` pattern="tests_for" to check coverage.
