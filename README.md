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

### Planning & Design
- **brainstorming** — Collaborative idea-to-design process with incremental validation
- **writing-plans** — Creates comprehensive implementation plans with dependency tracking
- **executing-plans** — Executes pre-written plans with Task-based tracking and review checkpoints

### Development Workflows
- **test-driven-development** — Red-Green-Refactor cycle enforced before any implementation
- **subagent-driven-development** — Dispatches fresh subagents per task with two-stage review (spec + quality)
- **dispatching-parallel-agents** — Parallel agent dispatch for independent problem domains
- **systematic-debugging** — Four-phase root cause investigation before attempting fixes

### Code Review
- **requesting-code-review** — Structured code review requests with severity-based response protocol
- **receiving-code-review** — Technical evaluation of review feedback without performative agreement

### Git & Branch Management
- **using-git-worktrees** — Isolated workspace creation with smart directory selection and safety verification
- **finishing-a-development-branch** — Structured completion options: merge, PR, keep, or discard

### Quality & Meta
- **verification-before-completion** — Evidence-based verification before any completion claims
- **writing-skills** — TDD-based approach to creating and testing new skills
- **using-coca** — Skill discovery and invocation protocol for every conversation
