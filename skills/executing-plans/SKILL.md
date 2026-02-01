---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints
---

# Executing Plans

## Overview

Load plan, review critically, execute tasks in batches, report for review between batches.

**Core principle:** Batch execution with checkpoints for architect review.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

## The Process

### Step 1: Load and Review Plan
1. Read plan file
2. Review critically - identify any questions or concerns about the plan
3. If concerns: Raise them with your human partner before starting
4. If no concerns: Create tasks using TaskCreate (one per plan task, with description carrying full context and activeForm in present continuous). Wire dependencies using TaskUpdate `addBlockedBy` based on each task's **Depends on** line. Then proceed.

### Step 2: Create Branch

Before making any changes, create an isolated branch for this work:

- Announce: "I'm using the using-git-worktrees skill to set up an isolated workspace."
- **REQUIRED SUB-SKILL:** Use coca-wits:using-git-worktrees
- Follow that skill to select directory, verify safety, create worktree, and confirm clean baseline

**Do not proceed to execution until the branch is ready and tests pass.**

### Step 3: Execute Batch
**Default: First 3 unblocked tasks** (use TaskList to find tasks with empty `blockedBy`)

For each task:
1. TaskGet to read full description and verify `blockedBy` is empty
2. TaskUpdate status to `in_progress` (triggers activeForm spinner)
3. Follow each step exactly (plan has bite-sized steps)
4. **Use proper file tools** (see File Tool Rules below)
5. Run verifications as specified
6. TaskUpdate status to `completed` (automatically unblocks dependent tasks)

### Step 4: Report
When batch complete:
- Show what was implemented
- Show verification output
- Say: "Ready for feedback."

### Step 5: Continue
Based on feedback:
- Apply changes if needed
- Execute next batch
- Repeat until complete

### Step 6: Complete Development

After all tasks complete and verified:
- Announce: "I'm using the finishing-a-development-branch skill to complete this work."
- **REQUIRED SUB-SKILL:** Use coca-wits:finishing-a-development-branch
- Follow that skill to verify tests, present options, execute choice

## When to Stop and Ask for Help

**STOP executing immediately when:**
- Hit a blocker mid-batch (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- You don't understand an instruction
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## When to Revisit Earlier Steps

**Return to Review (Step 1) when:**
- Partner updates the plan based on your feedback
- Fundamental approach needs rethinking

**Don't force through blockers** - stop and ask.

## File Tool Rules

**NEVER use Bash for file operations.** Use dedicated tools:

| Operation | Correct Tool | WRONG (never use) |
|-----------|-------------|-------------------|
| Read file | `Read` | `cat`, `head`, `tail`, `less` |
| Edit existing file | `Edit` | `sed`, `awk`, `perl -pi` |
| Create new file | `Write` | `echo >`, `cat <<EOF >`, `tee` |
| Search file contents | `Grep` | `grep`, `rg` via Bash |
| Find files by pattern | `Glob` | `find`, `ls` via Bash |

**Reserve Bash exclusively for:** running tests, builds, git commands, and other actual system operations.

## Remember
- Review plan critically first
- Follow plan steps exactly
- Don't skip verifications
- Use proper file tools (Edit, Read, Write) — never sed/awk via Bash
- Reference skills when plan says to
- Between batches: just report and wait
- Stop when blocked, don't guess
