# AGENTS.md: the instruction layer for agentic repositories

This repository pitches, publishes, and maintains a canonical `AGENTS.md`: a source-of-truth instruction file for AI agents and human maintainers.

If a repository expects agents to do useful work, it needs more than prompts in a chat window. It needs local, versioned operating instructions that explain how work is supposed to happen in that repository. That is what `AGENTS.md` is for.

## What AGENTS.md is

`AGENTS.md` is a repository-level instruction document.

It is meant to be read before work begins, then followed while planning, editing, reviewing, and verifying changes. A good `AGENTS.md` turns implicit maintainer expectations into explicit, versioned instructions.

In this repository, `AGENTS.md` is the canonical document. The other files explain the repository, contribution process, conduct baseline, security reporting, and pull request expectations. They do not replace or override `AGENTS.md`.

## The pitch

AI-assisted work gets messy when operating rules live in chat history, private memory, scattered notes, or maintainer assumptions. Agents guess. Humans correct the same mistakes repeatedly. Reviewers cannot tell whether an agent followed the intended workflow because the workflow was never written down.

`AGENTS.md` fixes that by making repository expectations explicit:

- **One visible contract** — agents and humans inspect the same instructions.
- **Versioned behavior** — every rule change is a normal repository diff.
- **Better onboarding** — a new agent can start with local context instead of generic defaults.
- **Safer automation** — boundaries, verification habits, and architectural constraints are written before work begins.
- **Less drift** — obsolete or conflicting guidance can be removed at the source.

## Common use cases

Use an `AGENTS.md` file when you need to:

- **Onboard coding agents** with local expectations before they touch files.
- **Document repository workflow** such as planning, editing, verification, and review habits.
- **Encode architectural constraints** so changes preserve the project's design intent.
- **Make automation auditable** by storing agent instructions as versioned text.
- **Align human and AI contributors** around the same source of truth.
- **Clarify contribution norms** without relying on one-off comments or private prompts.
- **Reduce drift** between what maintainers expect and what agents actually do.

## Repository contents

- `AGENTS.md` — the canonical instruction file and source of truth for this repository.
- `README.md` — the human-facing explanation of what the repository is for.
- `CONTRIBUTING.md` — how to propose changes to `AGENTS.md` and its support documents.
- `SECURITY.md` — how to report sensitive issues privately.
- `CODE_OF_CONDUCT.md` — the collaboration baseline for public participation.
- `LICENSE` — the license for the repository contents.
- `.github/PULL_REQUEST_TEMPLATE.md` — review prompts for scoped AGENTS.md-centered changes.
- `.editorconfig` and `.gitattributes` — repository formatting and line-ending conventions.

## How to use this repository

### If you are reading the rules

Read `AGENTS.md` first. It is the authoritative file.

Use this README when you need the purpose, scope, and intended use cases. Use `CONTRIBUTING.md` when you want to change the repository.

### If you are adopting AGENTS.md elsewhere

Copy deliberately, not blindly.

1. Read the whole `AGENTS.md` file.
2. Keep rules that match your repository's actual workflow.
3. Remove rules that do not apply.
4. Add project-specific constraints only when they are enforceable and reviewable.
5. Keep the adapted file as the source of truth in the target repository.
6. Avoid creating duplicate policy documents that compete with it.

A useful `AGENTS.md` is specific enough to guide real work and small enough to maintain.

## Maintenance principles

- `AGENTS.md` remains canonical.
- Support documents explain the repository and process; they do not add competing instructions.
- Changes should be focused, justified, and easy to review.
- Prefer clearer wording over more rules.
- Delete obsolete guidance instead of preserving compatibility shims.
- Keep examples and explanations tied to real repository behavior.

## Out of scope

This repository is not:

- an AI framework,
- a software package,
- a prompt marketplace,
- a generic policy stack,
- a collection of unrelated agent templates,
- or a manifesto about all possible AI workflows.

It is the `AGENTS.md` file, its purpose, its use cases, and the small set of documents needed to maintain it well.
