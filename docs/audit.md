# Audit Trail

## 2026-07-10: MCP Tools Stress-Test & AGENTS.md Update

### Summary
Ran a comprehensive stress-test across all discoverable MCP tools and
built-in capabilities. Expanded the AGENTS.md MCP section from a single
code-review-graph reference to a full capability-based tool framework
with fallback chains, decision tree, and gitignore boundary guidance.

### Files/Areas Touched
- `C:\Users\zznabil\.claude\CLAUDE.md` — replaced `## MCP Tools: code-review-graph`
  (lines 499-537) with `## MCP Tools & Built-in Capabilities` (~200 lines)
- `C:\Users\zznabil\Documents\agents.md\agent-foundry\.gitignore` — created to
  isolate disposable `.workflow/` artifacts
- `C:\Users\zznabil\Documents\agents.md\agent-foundry\.workflow\ultracode\mcp-stress-test\`
  — full run directory with plan, orchestration, state, capability matrix, mock code
- `docs/audit.md` — this file (created)

### Reason
The AGENTS.md tool guidance was stuck at a narrow code-review-graph section,
leaving all other MCP servers undocumented. With ~15+ MCP families available,
agents had no systematic fallback guidance. This update provides capability-based
tool selection regardless of which servers are available in a given session.

### Stress-Test Design
A disposable mock codebase (9 files, 848 lines across 5 languages, YAML config, and parser-invalid fixtures) was created in `.workflow/ultracode/mcp-stress-test/mock-code/` and used to probe tool capabilities:
**Representative tool-family smoke tests (18 MCP server families + built-in LSP):**
1. code-review-graph — semantic search, detect_changes, review context, suggested questions
2. octocode/oqlsearch — code search, structure, semantics, content (filesystem-path based)
3. repomix — pack_codebase (gitignore-bound even with includePatterns override)
4. tavily — web search for current MCP best practices
5. parallel-search — multi-query web research
6. microsoft-learn — official MCP integration documentation
7. everything-search — v3 (1.5.0.1404) available, filename + content search
8. deepwiki — unavailable (repo not indexed on deepwiki.com)
9. engram — fresh system (0 sessions), tools working
10. node_repl — Node.js runtime (process blocked, os/crypto/etc available)
11. squeez — compression protocol + session stats working
12. codesight — AI context map scan working
13. context-engine — context_search needs indexed content first
14. context-mode — ctx_stats, ctx_search, ctx_execute all working
15. codebase-memory — traced paths, graph schema, search (project indexed)
16. codegraph — unavailable (no .codegraph/ directory)
17. probe — failed (model API not configured for gpt-5.2)
18. cce-imgui-* — project-specific (Dear ImGui), available but unrelated
19. lsp — diagnostics on mock code (parser-invalid files correctly flagged)

### Key Findings Absorbed into Policy
- MCP availability is session-dependent → write capability-based fallbacks, not
  hardcoded tool names
- Gitignore is the primary scope boundary: code-review-graph + repomix respect it;
  grep/ast_grep/read/lsp/octocode do not
- Decision tree: graph first for *understanding*, grep first for *known targets*
- Content search hierarchy: grep (fast) → ast_grep (structural) → octocode (multi-modal)
- Web research: tavily (fast) → parallel-search (broad) → microsoft-learn (authoritative)
- All unverified claims marked with "verify" or "if available"

### Risk Assessment
- **Low risk** for production files (AGENTS.md update is additive, policy guidance)
- Mock code is fully disposable and gitignored
- Workflow artifacts are gitignored
- No production code was modified

### Verification Performed
- Re-read final AGENTS.md section for duplication, consistency, and internal conflicts
- Cross-checked decision tree vs prose sections for alignment
- Confirmed gitignore boundary claims match observed behavior
- Confirmed all tool categories have fallback chains

---

## 2026-07-10: Capability-First Conversion (post-review)

### Summary
User reviewed the initial MCP update and identified that pre-existing
sections of AGENTS.md still contained hardcoded tool preferences (notably
§9 Tooling and Editing Discipline's "Preferred tools" list, and a
`codegraph` reference in the §1 Shared Change section). Converted every
remaining hardcoded tool directive to capability-based language.

### Files/Areas Touched
- `C:\Users\zznabil\.claude\CLAUDE.md` §9 (lines 209-248) — replaced "Preferred
  tools" named-list with a capability category table. Examples in parens,
  capability in bold. Removed "If a preferred tool is unavailable" paragraph.
  Reworded "Mandatory habits" to remove tool-specific directives.
- `C:\Users\zznabil\.claude\CLAUDE.md` line 58 — `use LSP, codegraph, AST tools`
  → `use structural code intelligence tools (LSP, AST parsers, code graphs)`
- `C:\Users\zznabil\.claude\CLAUDE.md` line 614 — `Use grep, ast_grep, or octocode`
  → `Use filesystem-path tools (grep, ast_grep, octocode)`

### Reason
User rejected all hardcoded tool preferences. The new principle is:
describe the *capability* first and *what you're trying to do*; tool names
appear only as parenthetical examples. This survives session-to-session
tool availability changes.

### Verification Performed
- `grep`-swept for `(use|prefer|always|should|must) .* (<tool-name>)` patterns
  across the entire document — zero remaining matches.
- `grep`-swept for `(must|should|shall) .* (<tool-name>)` — zero matches.
- Confirmed every directive is now capability-based with optional examples.

## 2026-07-11: Canonical Synchronization — AGENTS.md ← CLAUDE.md

### Summary
User declared CLAUDE.md as the canonical source ("claude is the canon").
Replaced repo AGENTS.md verbatim with CLAUDE.md content via filesystem copy.
Resolves divergence where CLAUDE.md had capability-first tool guidance
while AGENTS.md retained the old hardcoded "Preferred tools" list and
narrow code-review-graph section.

### Files/Areas Touched
- `C:\Users\zznabil\Documents\agents.md\agent-foundry\AGENTS.md` — full replace
  with CLAUDE.md content (verified byte-identical: `4f9d83d4`)
- `docs/audit.md` — this entry

### Reason
Repo AGENTS.md and global CLAUDE.md had diverged: CLAUDE.md had the
capability-first framework (expanded MCP Tools section, proactive discovery
step 0, gitignore boundary, capability selection table), while AGENTS.md
still had the old "Preferred tools" list with hardcoded tool names and the
narrow "MCP Tools: code-review-graph" section. User's directive was
"claude is the canon" → full replace.

### Risk Assessment
- **Reversible** — git history preserves pre-overwrite AGENTS.md.
- **Scope** — one governance file, no code or schemas affected.

### Verification Performed
- `cp` filesystem copy, not manual reconstruction (avoids pagination gaps)
- md5sum hash match: `4f9d83d4f79fade3004ce8d3fdd257cf` on both files
- grep sweep for old hardcoded patterns ("\*\*codegraph\*\*: use when",
  "Use `grep`", "LSP for navigation") — all zero matches in new file

## 2026-07-11: Governance Trim — AGENTS.md 721→366 lines

### Summary
Trimmed AGENTS.md from ~721 to 366 lines (~49% reduction) to reduce per-turn
context pressure. Removed content duplicating system prompt (SOLID textbook,
capability table, §16 Considerate Agency subsections) and session-dependent
MCP catalog (decision tree, per-server descriptions, gitignore boundary).
Preserved core project-specific safeguards: task tiers, architecture mandate,
graph-first mandate with git-scope fallback, canonical filename map.

### Files/Areas Touched
- `AGENTS.md` — §6, §7, §9, §13, §16, and MCP/Tooling section replaced

### Reason
721 lines is too large for an agent to carry every turn. The old file contained
~400 lines of textbook-level definitions, session-dependent tool catalogs, and
content that substantially duplicated the system prompt's delivery contract
and tool policy.

### Safeguards Preserved
- Graph-first mandate retained with explicit git-scope fallback (target is
  untracked/gitignored/generated → filesystem tools even if graph is available)
- §16 pre-mortem and consultation requirements restored after initial cut
- Canonical filename map retained as compact table for first-creation reference

### Verification Performed
- Re-read every changed section and section boundary in the final file
- `wc -l` confirms 366 lines
- Confirmed no orphan legacy content (decision tree, per-server catalog,
  Gitignore Boundary, Session Dependency Warning) survived
- Verified heading boundaries between all trimmed sections
