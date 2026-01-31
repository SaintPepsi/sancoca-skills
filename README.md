# sancoca-skills

Personal Claude Code plugin with custom skill overrides.

## Installation

Add as a marketplace in Claude Code:

```
/install-plugin sancoca-skills@sancoca-skills
```

Or manually add the git URL as a marketplace source.

## Skills

- **executing-plans** — Uses TaskCreate/TaskUpdate/TaskList instead of TodoWrite, with dependency tracking via `addBlockedBy`
- **writing-plans** — Adds `Depends on` field to task templates for dependency wiring during execution
