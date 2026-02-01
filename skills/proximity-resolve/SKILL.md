---
name: proximity-resolve
description: Use when you have a proximity audit file to resolve — works through findings in batches with review checkpoints
---

# Proximity Resolve

## Overview

Consume a proximity audit findings file and apply fixes in batches with review checkpoints between each batch.

**Announce at start:** "I'm using the proximity-resolve skill to work through the audit findings."

**REQUIRED BACKGROUND:** You MUST understand proximity-principles before using this skill.

## The Process

### Step 1: Load and Review Audit

1. Read the audit file (produced by proximity-audit)
2. Create a task per finding
3. Group into batches by principle (related fixes are easier to review together)
4. Present summary: "Found N findings across M principles. Starting with [principle] (K findings)."
5. If any findings seem incorrect or questionable, raise concerns before starting

### Step 2: Execute Batch

Default batch size: one principle's findings.

For each finding:
1. Mark task in-progress
2. Use `Read` to read the file at the specified location
3. Apply the fix using `Edit` for existing files or `Write` for new files
4. Fix based on violation type:

| Principle | Fix action |
|-----------|-----------|
| Decision Archaeology | Add decision comment (why, alternatives, measurements) |
| Three-Strikes | Extract shared abstraction, update all call sites |
| Test Colocation | Move test file next to source, update imports |
| Error Context | Enrich error message with specific context at source |
| Config Near Usage | Move constant to usage site with rationale comment |
| Behavior Over Structure | **Proposal only** — describe restructuring, flag for human review |
| Temporal Proximity | **Proposal only** — describe restructuring, flag for human review |
| Security Annotations | Add security annotation with threat model and rationale |
| Performance Annotations | Add performance annotation with benchmarks and tradeoffs |
| Deprecation | Add deprecation notice with migration path and removal timeline |

5. Mark task completed

**Restructuring safety:** Behavior Over Structure and Temporal Proximity findings produce written proposals only. Never auto-apply file moves or directory restructuring. These require human decision.

### Step 3: Report

After completing a batch:
- Show what was changed (files modified, type of fix)
- Show any findings flagged for human judgment
- Say: "Ready for feedback."

### Step 4: Continue

Based on feedback:
- Apply requested changes
- Move to next batch (next principle)
- Repeat until all findings are resolved or deferred

### Step 5: Final Summary

After all batches:
- Count of findings resolved
- Count of findings deferred (proposals needing human decision)
- List of deferred proposals for follow-up

## When to Stop and Ask

- **Finding seems wrong** — If a finding doesn't look like a real violation on closer inspection, skip it and note why
- **Fix would break things** — If applying a fix would change behavior or break tests, stop and ask
- **Unclear context** — If you can't determine the right decision comment or rationale, ask the user rather than guessing
- **Large restructuring** — Any change touching more than 5 files should be confirmed before applying
