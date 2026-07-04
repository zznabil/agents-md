# AGENTS.md — Single Source of Truth

This file is the only governance document for any project under this directory. Do not create parallel policy docs. If a rule changes, update this file and retire the old source.

## 1) Architecture Mandate

Any project under this directory follows **functional core / imperative shell**.

- **Pure functions are the default.** Domain logic must be deterministic, side-effect free, and easy to test.
- **State lives in plain data containers.** Keep mutable state isolated in small, explicit structs/classes/dataclasses/records with no business logic.
- **I/O belongs in the imperative shell only.** UI, filesystem, network, input devices, timing, threads, and persistence stay at the edges.
- **Minimal OOP is allowed only when it is the pragmatic solution.** Use classes for lifecycle, adapters, UI shells, or resource boundaries; keep them thin and boring.
- **Composition over inheritance.** Prefer function composition and object composition. Avoid inheritance hierarchies for domain behavior.
- **No hidden global state.** Make dependencies explicit and pass them in.

## 2) Determinism & Reproducibility

Make behavior explicit, deterministic, and reproducible unless a boundary concern requires otherwise.

- Domain logic should be deterministic whenever practical.
- Functions should depend only on explicit inputs.
- Avoid implicit dependencies on time, randomness, environment variables, or global state unless they are injected at the boundary.
- Isolate nondeterministic operations — clock, UUID generation, randomness, network, filesystem — behind interfaces or adapters.
- Prefer reproducible builds, tests, and tooling.
- Preserve deterministic behavior in the functional core during all changes, including AI-assisted edits.

## 3) Strategy Behavioral Pattern

Pluggable behavior must use the **strategy pattern**.

- New variants are added by writing a new function/object and registering it in the appropriate registry or dispatch table.
- Do **not** hard-code variant selection into the orchestrator when a strategy seam exists.
- Strategy interfaces must be narrow, explicit, and stable.
- Do not smuggle a generic `config` blob through strategy boundaries. Pass only what the strategy actually needs.
- Keep strategy registration centralized and obvious.

## 4) SOLID, Separation of Concerns, and Modularity

Treat these as active engineering constraints, not slogans.

- **Single Responsibility:** one module, one job.
- **Open/Closed:** extend by adding a strategy or pure helper, not by rewriting the core.
- **Liskov Substitution:** every strategy must honor the exact contract it advertises.
- **Interface Segregation:** small interfaces, small inputs, small outputs.
- **Dependency Inversion:** high-level code depends on abstractions and function references, not concrete implementations.
- **Great separation of concerns:** keep orchestration, domain logic, persistence, UI, and integration boundaries distinct.
- **High cohesion:** related behavior stays together; unrelated behavior is split out.
- **Low coupling:** minimize cross-module knowledge and direct dependencies.
- **High composability:** prefer small helpers that combine cleanly.
- **High modularity / componentization:** build reusable units with sharp boundaries.
- **Easy reusability and extensibility:** design for adding new behavior without broad rewrites.

## 5) Code Smells Are Not Acceptable

The following are treated as defects that must be removed, not tolerated:

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

## 6) Developer Debugging Experience

Optimize for fast diagnosis.

- Favor deterministic logic and explicit inputs/outputs.
- Keep errors clear, contextual, and actionable.
- Avoid silent failures, swallowed exceptions, and ambiguous fallbacks.
- Use small functions so failures are easy to isolate.
- Keep state transitions observable.
- Prefer narrow, testable seams over clever abstraction.
- Preserve reproducibility: if a bug happens once, make it easy to reproduce again.

## 7) Tooling Is Mandatory and Must Be Aggressive

Use the best tool available, not the easiest text hack.

### Required workflow for non-trivial work

1. **codegraph** first when an index exists for the current project root. Use it to understand symbols, call paths, and blast radius before changing shared code.
2. **LSP** for navigation, references, rename, diagnostics, and refactors whenever a language server is available.
3. **AST tools** (`ast_grep`, `ast_edit`) for structural discovery and structural rewrites. Use them for declarations, callsites, signatures, imports, and codemods.
4. **Hashline edit** for file modifications when editing by line is the right tool. Use snapshot-tagged, line-accurate edits instead of broad text replacement.
5. **Plain text search/edit** only when the better tool is unavailable or clearly insufficient.

### Mandatory habits

- Use **codegraph, LSP, AST tools, and hashline edit proactively and frequently**, not as an afterthought.
- Before renaming an exported symbol, run **LSP references** first.
- Before editing shared or central code, inspect dependencies and call paths.
- After every significant edit, re-ground on the updated file state before continuing.
- Prefer the smallest correct change.
- Never use text search/replace to perform a structural change when AST, LSP, or hashline can do it safely.

## 8) Implementation Style

- Favor pure functions over imperative code whenever possible.
- Use classes only when they add real value at the shell or boundary layer.
- Keep object methods small and compositional.
- Avoid inheritance except where a framework forces it or a boundary object is genuinely simpler that way.
- Delete dead code, unused shims, and temporary compatibility layers once migration is complete.
- Keep modules focused and boring.

## 9) Source-of-Truth Discipline

- This file supersedes any older governance or practices document in this directory.
- Do not introduce competing instructions in another markdown file.
- If a future rule conflicts with this file, update this file rather than layering a second policy doc on top.
