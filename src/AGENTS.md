# AI SDLC Plugin for GitHub Copilot

This plugin provides AI-powered Software Development Lifecycle (SDLC) capabilities for GitHub Copilot projects, with a focus on Copilot CLI workflows.

## Purpose

The AI SDLC plugin helps teams streamline software development workflows by providing:

- **Workflow Commands**: command entrypoints for common SDLC tasks like feature development, bug fixes, and code reviews
- **Specialized Agents**: AI agents optimized for development tasks (spec writing, implementation, verification)
- **Skills**: reusable capabilities for standards, documentation, and workflow orchestration
- **Coding Standards**: project-level standards and best practices that can be customized and enforced

## Critical Principle: User-Confirmed Rollback

**NEVER automatically rollback or revert code changes without user confirmation.**

All workflows follow this pattern when failures occur:

1. **STOP** - Do not attempt automatic rollback for critical failures.
2. **ANALYZE** - Examine root cause (configuration, test setup, or logic issue).
3. **CHECK FOR EASY FIXES** - Many failures are simple setup/configuration issues.
4. **ASK USER** - Use `ask_user` with options:
   - "Try suggested fix" (if easy fix identified)
   - "Rollback changes" (user confirms rollback)
   - "Let me investigate" (pause for manual investigation)
5. **EXECUTE** - Only rollback if user explicitly confirms.

## Workflow Types Supported

This plugin supports 6 workflow types routed to specialized orchestrators:

| Workflow Type | Purpose | Orchestrator | Classification Keywords |
|---------------|---------|-------------|------------------------|
| **Development** | Bug fixes, enhancements, new features | development | "fix", "bug", "add", "new", "improve", "enhance", "create" |
| **Performance** | Optimize speed/efficiency | performance | "slow", "optimize", "speed up", "faster" |
| **Migration** | Move tech/patterns | migration | "migrate", "move from X to Y", "upgrade" |
| **Research** | Investigate and document findings | research | "research", "investigate", "explore options" |
| **Product Design** | Design features/products before building | product-design | "design", "product design", "feature design", "wireframe", "prototype" |
| **Incident** | Respond to production incidents and outages | incident | "incident", "outage", "sev", "production down", "5xx spike", "degraded" |

### Design Principles

- **Adaptive Phases**: development phases activate from task characteristics, not fixed task labels.
- **Characteristic Detection**: gap analysis detects defect work, modifications, new capabilities, data operations, and UI changes.
- **Flexible Granularity**: complex steps can expand into substeps.
- **Consistent Core**: all workflows share analysis, specification, planning, implementation, and verification.
- **Conditional Stages**: optional stages activate from context (for example TDD gates or UI mockups).

## Terminology

**Development Task** (or "Task")
- High-level work item from start to finish.
- Located at `.flowbit/tasks/[workflow-type]/YYYY-MM-DD-task-name/`.
- Contains requirements, specification, implementation plan, and verification results.

**Implementation Step** (or "Implementation Task")
- Specific actionable step during implementation.
- Listed in `implementation/implementation-plan.md`.
- Example: "1.1 Create User model", "2.3 Write API endpoint".

**Key distinction**: development task = WHAT to build, implementation steps = HOW to build it.

## Documentation and Task Organization

### Separation of Concerns

**`.flowbit/docs/`** - stable reference documentation:
- project vision, roadmap, architecture, and standards

**`.flowbit/tasks/`** - active work items:
- task directories per workflow, with execution artifacts

### Core Project Structure

```text
.flowbit/
├── docs/
│   ├── INDEX.md
│   ├── project/
│   └── standards/
└── tasks/
    ├── development/
    ├── incidents/
    ├── performance/
    ├── migrations/
    ├── research/
    └── product-design/
```

### Task Directory Structure

```text
YYYY-MM-DD-task-name/
├── orchestrator-state.yml
├── analysis/
├── implementation/
│   ├── spec.md
│   ├── implementation-plan.md
│   └── work-log.md
├── verification/
└── documentation/ (optional)
```

### Naming Conventions

- Workflow directories: `development/`, `incidents/`, `performance/`, `migrations/`, `research/`, `product-design/`
- Task directories: `YYYY-MM-DD-task-name`

## Plugin Documentation Principles

These principles keep plugin documentation concise and maintainable while trusting the agent to reason:

1. **No verbose pseudocode** - document patterns and decisions, not full implementations.
2. **No prescriptive templates** - guide intent, avoid rigid scripts.
3. **Avoid duplication** - reference `SKILL.md` for technical details.
4. **Commands as thin wrappers** - command files should route; orchestration belongs in skills.
5. **Single source of truth** - keep orchestration logic in skills and references.
6. **Principle over process** - explain WHY/WHEN and let the agent determine HOW.

## Orchestrator Framework

Shared orchestrator references:

- `skills/orchestrator-framework/references/orchestrator-patterns.md`
- `skills/orchestrator-framework/references/orchestrator-creation-checklist.md`

Key expectations:
- state-driven execution
- resume support via `orchestrator-state.yml`
- interactive phase gates
- user-confirmed rollback
- explicit context passing between phases

## Available Skills

Core workflow skills:

- `codebase-analyzer`
- `implementation-verifier`
- `standards-discover`
- `docs-manager` (internal engine used by `docs-operator`)
- `init`
- `standards-update`
- `quick-bugfix`
- `incident-intake`
- `incident-triage`
- `incident-evidence`
- `incident-postmortem`

Primary orchestrator skills:

- `development`
- `performance`
- `migration`
- `research`
- `product-design`
- `incident`

See `skills/*/SKILL.md` for details.

## Available Commands

Setup/standards:

- `/flowbit:init`
- `/flowbit:standards-update`
- `/flowbit:standards-discover`

Workflow:

- `/flowbit:work`
- `/flowbit:development`
- `/flowbit:performance`
- `/flowbit:migration`
- `/flowbit:research`
- `/flowbit:product-design`
- `/flowbit:incident`

Reviews/audits:

- `/flowbit:reviews-code`
- `/flowbit:reviews-pragmatic`
- `/flowbit:reviews-spec-audit`
- `/flowbit:reviews-reality-check`
- `/flowbit:reviews-production-readiness`

Quick commands:

- `/flowbit:quick-plan`
- `/flowbit:quick-dev`
- `/flowbit:quick-bugfix`

## Available Agents

Commonly used agents include:

- analysis: `project-analyzer`, `task-classifier`, `gap-analyzer`, `specification-creator`, `implementation-planner`
- UI/docs: `ui-mockup-generator`, `e2e-test-verifier`, `user-docs-generator`
- performance: `bottleneck-analyzer`
- research: `research-planner`, `information-gatherer`, `research-synthesizer`, `solution-brainstormer`, `solution-designer`
- incident: `incident-triage`, `timeline-correlator`, `blast-radius-analyzer`, `mitigation-selector`, `postmortem-author`
- verification/review: `implementation-completeness-checker`, `test-suite-runner`, `code-reviewer`, `production-readiness-checker`, `code-quality-pragmatist`, `spec-auditor`, `reality-assessor`

See `agents/*.agent.md` for workflow-specific guidance.

## Key Workflow Principles

1. **Documentation first**: read `.flowbit/docs/INDEX.md` before implementation.
2. **Specification before code**: ensure clear scope.
3. **Planning before execution**: define implementation groups/steps.
4. **Test-driven approach** when task type requires it.
5. **Continuous standards discovery** throughout execution.
6. **Incremental verification** during implementation.
7. **Comprehensive verification before completion**.
8. **Artifact anchoring**: keep all generated artifacts inside `.flowbit/tasks/[type]/[task-name]/`.

## Progress Tracking

Track workflow and task-group progress through:

- `TaskCreate` for initializing phases and task groups
- `TaskUpdate` for state transitions (`pending` -> `in_progress` -> `completed`)
- `orchestrator-state.yml` as source of truth for resume and failure handling

## Hooks

Plugin hooks can enforce workflow constraints and safety controls.

Current hook examples:

- state reminder hook (`hooks/post-compact-reminder.sh`)
- destructive command protection (`hooks/block-destructive-commands.sh`)

When adding hooks, ensure behavior aligns with user-confirmed rollback and non-destructive defaults.

## GitHub Copilot Documentation (Authoritative References)

Always prefer current GitHub documentation when updating plugin behavior:

1. **Copilot CLI plugin reference**  
   <https://docs.github.com/copilot/reference/copilot-cli-reference/cli-plugin-reference>
2. **Copilot CLI command reference**  
   <https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference>
3. **Find and install Copilot CLI plugins**  
   <https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/plugins-finding-installing>
4. **Add skills for Copilot CLI**  
   <https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-skills>
5. **Create custom agents for Copilot CLI**  
   <https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli>
6. **Custom instructions support matrix**  
   <https://docs.github.com/en/copilot/reference/custom-instructions-support>

Documentation priority:
1) Official GitHub docs  
2) Project instructions (`AGENTS.md`, `.github/copilot-instructions.md`, `.flowbit/docs/`)  
3) Existing repository patterns  
4) General best practices

## Platform Notes: Copilot CLI

For this repository, use Copilot CLI conventions:

- Use sequential single-select prompts when multiple decisions are needed.
- Treat `AGENTS.md` as agent instruction source for Copilot CLI.
- Keep repository-wide behavior in `.github/copilot-instructions.md`.
- Keep plugin metadata in `plugin.json`.
- Use command and skill names exactly as exposed by plugin routing in Copilot CLI.

