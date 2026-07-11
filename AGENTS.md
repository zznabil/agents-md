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
- use structural code intelligence tools (LSP, AST parsers, code graphs) when they materially improve safety;
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

Treat these as practical engineering constraints, not slogans. Keep modules focused, composable, and loosely coupled; extract when cohesion weakens.

Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion, Separation of Concerns, High Cohesion, Low Coupling, Composability, Modularity.

## 7) Code Smell Response

Treat code smells as defects in production code, but respond proportionally. Extract growing functions, split grab-bag modules, centralize duplicated logic, break dependency cycles at the seam, name magic constants, delete dead code.

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

Use the best available tool for the task, matched to the risk tier. Choose tools by what they do, not by their name. If the strongest tool is unavailable, fall back to the next safest method that achieves the same goal. Note the substitution transparently in the final summary when it materially affects confidence or quality.

### Mandatory habits

- Before renaming an exported symbol, inspect references first.
- Before editing shared or central code, inspect dependencies and call paths.
- After every significant edit, re-ground on the updated file state before continuing.
- Prefer the smallest correct change.
- Do not use broad text replacement for structural code changes when structural tools (AST, LSP, code graph) are available and appropriate.

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

### Canonical locations

Maintain active project context at root `context.md`. Maintain remaining documentation under `docs/` with canonical lowercase filenames:

| File | Purpose |
|------|---------|
| `context.md` | Current project state, constraints, glossary, active scope |
| `docs/user-stories.md` | User goals, roles, stories, product expectations |
| `docs/acceptance-criteria.md` | Testable done conditions, verification checklist |
| `docs/architecture-diagram.md` | High-level architecture, components, boundaries |
| `docs/block-diagram.md` | Block-level component decomposition |
| `docs/sequence-diagrams.md` | Mermaid sequence diagrams for important flows |
| `docs/activity-diagrams.md` | Activity diagrams for workflows and processes |
| `docs/flowcharts.md` | Procedural decision flows |
| `docs/state-machines.md` | State machines, transitions, guards, side effects |
| `docs/use-case-diagram.md` | Actors, use cases, system boundary |
| `docs/event-flow-diagram.md` | Domain events, producers, consumers, ordering |
| `docs/uml.md` | UML index (do not duplicate other doc content) |
| `docs/adr/` | Architecture Decision Records, one per file |
| `docs/changelog.md` | User-visible or externally meaningful changes |
| `docs/audit.md` | Engineering change trace with context |

Not every project must instantiate every document — create when useful, never create empty placeholders. ADRs go in `docs/adr/`. Do not duplicate context across root and `docs/`. Do not mix cases (prefer lowercase canonical names; migrate old uppercase variants if they exist).

### Update rule

Update affected docs in the same change when architecture, user-visible behavior, state transitions, APIs, major decisions, or risks change. If a change has no documentation impact, leave docs untouched.

### Diagrams

Prefer Mermaid inside Markdown. Diagrams must describe the current system. Do not maintain diagrams for flows too trivial to justify them.

### ADRs

Use ADRs for durable decisions (architecture, dependencies, data model, interfaces, deployment, security, performance). Each ADR: status, context, decision, consequences, alternatives considered. Do not ADR trivial details.

### Audit and changelog

`docs/audit.md` records meaningful engineering changes with context. `docs/changelog.md` records externally meaningful changes. Update only when relevant; do not turn into commit logs.

### Staleness prevention

Before editing code, check existing docs. Update after, remove contradictions, ensure docs and implementation tell the same story. If they conflict, resolve by checking code, tests, user request, and intent before changing either side.

### Anti-bloat guardrails

Do not create empty placeholder docs, duplicate diagrams, or write governance into `docs/`. Keep docs close to the system and prune aggressively.

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

Agents should reduce the need for human supervision through considerate, context-aware judgment. Complete the intended outcome, not merely the literal task, while preserving the user's authority over consequential choices.

### Aim

Optimize for intent fidelity, right-sized completion, and system integrity. Anticipate ordinary needs, follow through on safe work, and avoid making the user manage routine decisions or recover from preventable omissions.

Before shared, architectural, or risky work, run a brief pre-mortem covering intent mismatch, regression or blast radius, scope creep, and verification gaps. Consult an advisor before an irreversible, high-blast-radius, or security-sensitive decision. Exhaust safe in-scope alternatives before escalating to the user.

### Finish for the Human

Start every completion with the outcome. For non-trivial work, state the material assumptions or decisions, verification actually performed, affected areas, and any remaining risk or user choice. Omit routine tool narration and implementation trivia unless it changes a decision. End with a compact TL;DR.

## 17) Final Work Summary

For non-trivial work, finish with a concise summary covering:

- what changed;
- files or areas touched;
- tests, checks, or verification performed;
- documentation updated or intentionally left unchanged;
- risks, limitations, or follow-up work that matters.

Do not claim certainty beyond the evidence. Be explicit about anything not run, not checked, or intentionally deferred.

## Tooling & Discovery

This project uses a capability-first approach: choose the strongest available tool for each task, not the most familiar name. MCP servers are session-dependent — a server present today may be absent tomorrow.

### Graph-first mandate

If the code-review-graph MCP server is available (detected via `search_tool_bm25` or active tool list), use its structural graph tools **before** Grep/Glob/Read for codebase exploration:

- `query_graph` / `semantic_search_nodes` for callers, callees, imports, tests
- `get_impact_radius` / `get_affected_flows` for blast radius
- `detect_changes` + `get_review_context` for code review

**Fallback when graph is unavailable or the target is untracked / gitignored / generated (outside the graph's git index):** use filesystem tools (grep, ast_grep, lsp, read). Rule of thumb: graph first for structural understanding; grep first for finding a known string in a known file.

### Proactive discovery

Call `search_tool_bm25` at session start with capabilities you need — it activates matching MCP tools. If a tool is absent, follow the fallback chain from strongest available to safest fallback. Document blocked tools as session limitations; do not silently substitute.
