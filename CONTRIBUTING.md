# Contributing to the AGENTS.md repository

This repository maintains one thing: the canonical `AGENTS.md` file and the support documents needed to explain, review, and maintain it.

`AGENTS.md` is the source of truth for repository operating rules. This file explains how to propose changes; it does not create a second rule system.

## What belongs here

Good contributions improve the usefulness, accuracy, or maintainability of `AGENTS.md` and its support files.

Acceptable changes include:

- clarifying ambiguous wording in `AGENTS.md`,
- adding a missing rule with a concrete rationale,
- removing obsolete, duplicated, or conflicting guidance,
- tightening instructions so agents and humans can follow them consistently,
- improving README explanations of purpose, use cases, and adoption,
- improving contribution, security, conduct, or pull request guidance,
- fixing typos, formatting, broken links, or inaccurate file references.

## What does not belong here

Do not add:

- unrelated source code, packages, binaries, demos, or generated artifacts,
- generic AI essays that do not improve this repository's `AGENTS.md`,
- duplicate governance documents that compete with `AGENTS.md`,
- speculative rules without a real workflow or maintenance reason,
- broad policy stacks that make the repository harder to understand,
- compatibility shims for obsolete text that should be deleted.

## Proposing a change

A useful proposal answers these questions:

1. What problem does the current wording create?
2. Which file and section are affected?
3. What behavior changes before and after the edit?
4. Why does the change belong in this repository?
5. What compatibility or interpretation risk does it introduce?
6. How did you check the final wording?

Keep changes focused. If two edits change unrelated rules or documents, split them.

## Review checklist for AGENTS.md changes

Before opening a pull request, check that the change:

- preserves `AGENTS.md` as the single source of truth,
- avoids duplicating the same rule in multiple files,
- uses deterministic, direct wording,
- states requirements clearly enough for an agent to follow,
- avoids hidden scope creep or unrelated policy expansion,
- removes obsolete text instead of layering around it,
- keeps support documents explanatory rather than authoritative.

## Formatting

Follow the repository formatting conventions:

- UTF-8 text,
- LF line endings,
- final newline at end of file,
- two-space indentation where indentation is needed,
- readable Markdown headings and lists.

Markdown files may keep intentional trailing spaces only when Markdown syntax requires them.
