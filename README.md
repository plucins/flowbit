# Flowbit Plugin for GitHub Copilot CLI

This repository contains the Flowbit GitHub Copilot CLI plugin under `src/`.

## What it provides

- SDLC workflow skills (development, performance, migration, research, product design, incident response)
- Specialized agents for analysis, planning, implementation, and verification
- Plugin bundle located in `src/` (`plugin.json`, `commands/`, `skills/`, `agents/`)

## Install from marketplace (recommended)

Add the Flowbit marketplace:

```bash
copilot plugin marketplace add plucins/flowbit
```

Browse available plugins:

```bash
copilot plugin marketplace browse flowbit
```

Install Flowbit from marketplace:

```bash
copilot plugin install flowbit@flowbit
```

## Install directly from repository

Install from plugin subdirectory:

```bash
copilot plugin install https://github.com/plucins/flowbit:src
```

Verify:

```bash
copilot plugin list
```

In an interactive Copilot CLI session, verify loaded components:

```text
/agent
/skills list
```

## Update after changes

After pushing changes, update the installed plugin:

```bash
copilot plugin update flowbit
```


## Getting started

### Step 1 — Initialize your project with `/flowbit:init`

Run this once per project to set up Flowbit's documentation and standards:

```text
/flowbit:init
```

The skill analyzes your codebase, asks a few questions, and creates the following structure:

```
.flowbit/
└── docs/
    ├── INDEX.md                  ← navigation index, loaded by Copilot on each turn
    ├── project/
    │   ├── architecture.md       ← auto-generated architecture doc
    │   ├── tech-stack.md         ← detected stack with rationale
    │   ├── vision.md             ← (optional, if selected)
    │   └── roadmap.md            ← (optional, if selected)
    └── standards/
        ├── global/               ← cross-cutting coding conventions
        ├── frontend/             ← (if frontend detected)
        ├── backend/              ← (if backend detected)
        └── testing/              ← testing patterns and rules
```

**Optional**: copy standards from another already-initialized project:

```text
/flowbit:init --standards-from=../other-project
```

---

### Step 2 — Start working with `/flowbit:work` (or a specific command)

#### Unified entry point (recommended)

```text
/flowbit:work
```

Describe your task in plain language. The `task-classifier` agent automatically routes it to the right workflow:

| Classification | Orchestrator invoked |
|---|---|
| Bug fix, feature, enhancement | `development` |
| Performance bottleneck | `performance` |
| Technology/version migration | `migration` |
| Investigation or spike | `research` |
| Feature design before coding | `product-design` |

#### Direct commands

You can also invoke a specific workflow directly:

```text
/flowbit:development   → feature work and bug fixes
/flowbit:performance   → bottleneck analysis and optimization
/flowbit:migration     → technology or architecture migrations
/flowbit:research      → investigation with documented findings
/flowbit:product-design → product/feature design sessions
```

#### What gets created per task

Every workflow creates a task folder and tracks progress. Example for a `development` task:

```
.flowbit/tasks/development/2026-05-23-add-auth/
├── orchestrator-state.yml
├── analysis/
│   ├── codebase-analysis.md       ← Phase 1: existing code review
│   ├── clarifications.md          ← Phase 1: open questions resolved
│   ├── gap-analysis.md            ← Phase 2: scope and integration points
│   └── ui-mockups.md              ← Phase 4: conditional, if UI-heavy
├── implementation/
│   ├── spec.md                    ← Phase 5: what will be built
│   ├── requirements.md            ← Phase 5
│   ├── implementation-plan.md     ← Phase 7: step-by-step plan
│   ├── work-log.md                ← Phase 8: execution log
│   ├── tdd-red-gate.md            ← Phase 3: failing tests (conditional)
│   └── tdd-green-gate.md          ← Phase 9: passing tests (conditional)
├── verification/
│   ├── spec-audit.md              ← Phase 6
│   └── implementation-verification.md ← Phase 11
└── documentation/
    └── user-guide.md              ← Phase 13: optional
```

The `orchestrator-state.yml` file tracks the current phase and lets you **resume** interrupted tasks:

```yaml
orchestrator:
  started_phase: implementation
  completed_phases: [analysis, gap_analysis, specification, planning]
  failed_phases: []
  options:
    e2e_enabled: false
    user_docs_enabled: null
    code_review_enabled: true
  task_context:
    risk_level: medium
    task_characteristics:
      has_reproducible_defect: false
      modifies_existing_code: true
      creates_new_entities: true
      involves_data_operations: true
      ui_heavy: false
  created: 2026-05-23T10:00:00Z
  updated: 2026-05-23T14:32:00Z
  task_path: .flowbit/tasks/development/2026-05-23-add-auth
task:
  title: Add authentication
  description: Implement JWT-based login and token refresh
  status: in_progress   # pending | in_progress | completed | failed
  priority: high
```

To resume an existing task, run `/flowbit:work` again — it detects the open task folder and continues from where it left off. You can also point to a specific task by name, e.g. `/flowbit:work 2026-05-23-add-auth`, matching the folder name under `.flowbit/tasks/`.

---