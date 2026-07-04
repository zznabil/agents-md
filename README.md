# Agent Foundry

A public, production-minded scaffold for autonomous coding-agent projects.

This repository starts with governance, conventions, and contributor hygiene first, so future implementation work lands in a stable, reviewable environment.

## What this repo is for

- A clean baseline for agent-oriented tools, workflows, and automation.
- A place to codify standards before code accumulates.
- A public reference point for maintainable, testable, and observable work.

## Principles

- Prefer small, deterministic units of behavior.
- Keep side effects at the edges.
- Make interfaces explicit.
- Delete dead paths instead of preserving compatibility by default.
- Verify changes with the smallest meaningful check that proves the behavior.

## Repository layout

- `README.md` — project overview and operating rules.
- `CONTRIBUTING.md` — change workflow and review expectations.
- `SECURITY.md` — vulnerability reporting guidance.
- `CODE_OF_CONDUCT.md` — collaboration baseline.
- `.github/` — pull request and issue templates.

## Contributing

1. Keep changes focused.
2. Explain the rationale for behavior changes.
3. Include verification evidence for anything non-trivial.
4. Prefer boring, readable implementations.

## Security

If you discover a vulnerability, report it privately through GitHub Security Advisories or another private maintainer channel. Do not open a public issue for sensitive reports.

## License

MIT. See `LICENSE`.