---
name: proximity-audit
description: Use when auditing a codebase for proximity violations — scattered decisions, separated tests, orphaned config, missing context
---

# Proximity Audit

## Overview

Systematically scan a codebase for proximity violations and produce a structured findings file. The findings file is consumed by proximity-resolve.

**Announce at start:** "I'm using the proximity-audit skill to audit this codebase."

**REQUIRED BACKGROUND:** You MUST understand proximity-principles before using this skill.

## The Process

### Step 1: Scope

Ask the user:
- Audit whole codebase, specific directory, or specific principles?
- Default: whole codebase, all 10 principles

### Step 2: Create Branch

Before making any changes, create an isolated branch for the audit work:

- Announce: "I'm using the using-git-worktrees skill to set up an isolated workspace."
- **REQUIRED SUB-SKILL:** Use coca-wits:using-git-worktrees
- Follow that skill to select directory, verify safety, create worktree, and confirm clean baseline

**Do not proceed to scanning until the branch is ready.**

### Step 3: Systematic Scan

Work through each principle using its detection strategy:

| Principle | What to look for |
|-----------|-----------------|
| Decision Archaeology | Magic numbers, unexplained constants, config values without rationale |
| Three-Strikes | 3+ near-identical code blocks that haven't been abstracted |
| Test Colocation | Test files in separate `test/` or `__tests__/` trees away from source |
| Error Context | Generic error messages ("error occurred", "something went wrong", bare rethrows) |
| Config Near Usage | Centralized config files with unrelated values bundled together |
| Behavior Over Structure | Top-level directories named by layer (`controllers/`, `models/`, `utils/`) |
| Temporal Proximity | Files that change together in git history but live far apart |
| Security Annotations | Security-sensitive code (sanitization, auth checks) without documented rationale |
| Performance Annotations | Optimizations (caching, batching, data structure choices) without benchmarks/reasoning |
| Deprecation | Deprecated code without migration paths or removal timelines |

**Detection techniques:**
- Use Grep/Glob for pattern-based detection (magic numbers, generic errors, test locations)
- Use `git log --pretty=format: --name-only` for temporal proximity analysis
- Use directory listing for structural analysis (layer-based organization)
- Read files to confirm violations before reporting (no false positives from grep alone)

### Step 4: Produce Findings File

Determine the current branch name with `git branch --show-current`.

Save to `docs/audits/<branch-name>/YYYY-MM-DD-proximity-audit.md`:

```markdown
# Proximity Audit — YYYY-MM-DD

**Scope:** [what was audited]
**Findings:** [total count] across [N] principles

## [Principle Name] (N findings)

### Finding 1
- **File:** `path/to/file.ext:LINE`
- **Violation:** [specific description of what's wrong]
- **Suggested fix:** [concrete action to take]

### Finding 2
...

## [Next Principle] (N findings)
...
```

**Rules for findings:**
- Always include file path and line number
- Describe the specific violation, not just the principle name
- Suggested fix must be concrete and actionable
- Skip principles with zero findings (don't include empty sections)
- Order principles by finding count (most violations first)
- Each finding belongs to exactly one principle — no "overlap" sections. Pick the most specific principle (e.g., Config Near Usage over Decision Archaeology when a constant is far from its consumer).

### Step 5: Summary

Print finding counts by principle and total. Then offer:

"Ready to resolve these? Use proximity-resolve with this audit file."

## When to Stop and Ask

- **Ambiguous violations** — If unsure whether something is a real violation or intentional, flag it as "potential" in the findings and note the ambiguity.
- **Massive codebase** — If the codebase is very large, propose auditing one module at a time rather than everything at once.
- **No findings** — If a principle has no violations, skip it. If the entire audit finds nothing, say so honestly.

## Integration

**Uses:**
- **using-git-worktrees** (Step 2) - REQUIRED to create isolated branch before audit work
- **finishing-a-development-branch** - REQUIRED after audit is complete to handle the branch

**Feeds into:**
- **proximity-resolve** - Consumes the findings file produced by this skill
