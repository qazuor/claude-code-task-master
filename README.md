# Task Master

End-to-end planning, specification, task management, and quality gating plugin for Claude Code.

## What It Does

Task Master brings structured project management to Claude Code:

- **Specification generation** - Formal specs from requirements (lite for medium features, full for complex ones)
- **Task management** - Atomic tasks with dependencies, complexity scores, and phases
- **Quality gates** - Automated lint/typecheck/test checks before task completion
- **Session continuity** - Resume where you left off across sessions
- **Overlap detection** - Prevents duplicate work across specs

## Installation

```bash
claude plugin add /path/to/claude-code-task-master
```

Or from GitHub:

```bash
claude plugin add github:qazuor/claude-code-task-master
```

## Quick Start

### Create a standalone task (simple work)

```
/new-task "Fix login button mobile responsiveness"
```

### Generate a specification (medium/complex features)

```
/spec "Add user authentication with Clerk"
```

This will:
1. Check for overlaps with existing specs
2. Assess complexity (medium or complex)
3. Enter Plan Mode for you to explore and plan
4. After approval, generate formal spec + tasks automatically

### View your dashboard

```
/tasks
```

### Start working on the next task

```
/next-task
```

### Check detailed progress

```
/task-status SPEC-001
```

### Re-plan when requirements change

```
/replan SPEC-001
```

## How It Works

### Flows by Complexity

**Simple** (bugfix, config change):
```
/new-task → implement → quality gate → done
```

**Medium** (new endpoint, component):
```
/spec → Plan Mode → approve → spec + tasks auto-generated → /next-task loop
```

**Complex** (multi-day feature, architecture):
```
/spec → Plan Mode → approve spec → tasks generated → approve tasks → /next-task loop
```

### Data Storage

All data is stored as JSON files in `.claude/` and committed to your repo:

```
.claude/
├── specs/                    # Formal specifications
│   ├── SPEC-001-feature/
│   │   ├── spec.md           # The spec document
│   │   └── metadata.json     # Parseable metadata
│   └── index.json            # Spec index
├── tasks/                    # Task state
│   ├── SPEC-001-feature/
│   │   ├── state.json        # Source of truth
│   │   └── TODOs.md          # Human-readable checklist
│   ├── standalone/
│   │   ├── state.json        # Tasks without specs
│   │   └── TODOs.md
│   └── index.json            # Global task index
└── plans/                    # Plan Mode files (Claude built-in)
```

### Task Lifecycle

```
pending → in-progress → quality gate → completed
                ↑              ↓
                └── fix ← FAIL
```

Tasks cannot be completed until quality gates pass (lint + typecheck + tests).

## Commands

| Command | Description |
|---------|-------------|
| `/spec` | Generate a specification from requirements |
| `/tasks` | View task dashboard with progress |
| `/next-task` | Get and start the next available task |
| `/new-task` | Create a standalone task (no spec needed) |
| `/task-status` | Detailed progress report for a spec or all work |
| `/replan` | Modify tasks when requirements change |

## Skills

| Skill | Description |
|-------|-------------|
| `spec-generator` | Generates formal spec docs from Plan Mode output |
| `task-atomizer` | Breaks features into atomic implementable tasks |
| `complexity-scorer` | Scores tasks 1-10 based on multiple factors |
| `dependency-grapher` | Validates dependency graph, finds critical path |
| `task-from-spec` | Orchestrates spec-to-tasks pipeline |
| `overlap-detector` | Detects overlaps with existing specs/tasks |
| `quality-gate` | Runs quality checks before task completion |

## Agents

| Agent | Description |
|-------|-------------|
| `spec-writer` | Writes functional specs (user stories, BDD criteria, UX) |
| `tech-analyzer` | Writes technical analysis (architecture, data, risks) |
| `task-planner` | Decomposes specs into tasks with dependencies |

## Configuration

### Custom Quality Gates

Create `.claude/task-master.config.json` in your project:

```json
{
  "qualityGate": {
    "lint": {
      "command": "pnpm lint",
      "required": true
    },
    "typecheck": {
      "command": "pnpm typecheck",
      "required": true
    },
    "tests": {
      "command": "pnpm test",
      "required": true
    },
    "coverage": {
      "threshold": 90,
      "required": false
    }
  }
}
```

### Project-Level Extensions

- **Custom agents**: Add domain-specific agents in your project's `.claude/agents/`
- **Custom templates**: Override templates in your project's `.claude/templates/`
- **Custom tags**: Define additional task tags relevant to your domain

## Session Resume

When you start a new Claude Code session with active tasks, the plugin automatically detects and shows:

```
Active Task Master work detected:

Active Epics:
  - SPEC-001: User Authentication (3/9 tasks, status: in-progress)

Standalone Tasks: 2 pending (1/3 completed)

Use /tasks for full dashboard, /next-task to continue working.
```

## Plugin Structure

```
claude-code-task-master/
├── .claude-plugin/
│   └── plugin.json           # Plugin manifest
├── commands/                  # 6 slash commands
│   ├── spec.md
│   ├── tasks.md
│   ├── next-task.md
│   ├── new-task.md
│   ├── task-status.md
│   └── replan.md
├── skills/                    # 7 skills
│   ├── spec-generator/
│   ├── task-atomizer/
│   ├── complexity-scorer/
│   ├── dependency-grapher/
│   ├── task-from-spec/
│   ├── overlap-detector/
│   └── quality-gate/
├── agents/                    # 3 agents
│   ├── spec-writer.md
│   ├── tech-analyzer.md
│   └── task-planner.md
├── templates/                 # Spec + JSON schema templates
│   ├── spec-lite.md
│   ├── spec-full.md
│   ├── state-schema.json
│   ├── metadata-schema.json
│   ├── index-schema.json
│   ├── specs-index-schema.json
│   └── config-example.json
├── hooks/
│   └── hooks.json             # SessionStart resume hook
├── scripts/
│   └── session-resume.sh      # Resume detection script
├── CHANGELOG.md
├── LICENSE
└── README.md
```

## License

MIT
