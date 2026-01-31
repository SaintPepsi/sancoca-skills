# sancoca-skills

Personal Claude Code plugin with custom skill overrides.

## Installation

1. Add the marketplace:
```
/plugin marketplace add SaintPepsi/sancoca-skills
```

2. Install the plugin:
```
/plugin install sancoca-skills@sancoca-skills
```

## Skills

- **executing-plans** — Uses TaskCreate/TaskUpdate/TaskList instead of TodoWrite, with dependency tracking via `addBlockedBy`
- **writing-plans** — Adds `Depends on` field to task templates for dependency wiring during execution
