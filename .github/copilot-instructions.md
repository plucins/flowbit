# Flowbit Plugin Instructions for GitHub Copilot CLI

Flowbit is an SDLC plugin for GitHub Copilot CLI. It provides structured workflows for planning, implementing, and verifying software changes through commands, skills, and specialized agents.

Use these instructions as the runtime behavior guide when this repository is loaded as a plugin.

## Mission

- Deliver predictable SDLC execution instead of ad hoc coding.
- Route work to the right workflow (`development`, `performance`, `migration`, `research`, `product-design`).
- Keep every workflow resumable, auditable, and artifact-driven.

## Plugin Structure

- `plugins/plugin.json`: plugin metadata and entrypoint configuration for commands/skills/agents.
- `plugins/commands/`: user-facing command entrypoints (routing wrappers).
- `plugins/skills/`: orchestration and reusable workflow logic.
- `plugins/agents/`: specialized subagents for focused tasks (analysis, planning, verification, etc.).
- `.flowbit/docs/`: project reference docs and standards (source of truth for conventions).
- `.flowbit/tasks/`: workflow task artifacts and execution history.

## Core Operating Rules

1. Read `.flowbit/docs/INDEX.md` first when available, then read applicable standards files before implementation decisions.
2. Never rollback, revert, or discard user changes without explicit user confirmation.
3. Prefer phased execution: analyze -> specify -> plan -> implement -> verify.
4. Keep all workflow artifacts under `.flowbit/tasks/<workflow-type>/<YYYY-MM-DD-task-name>/`.
5. Treat `orchestrator-state.yml` as state authority for resume/retry behavior.
6. Keep implementation steps distinct from the high-level task (WHAT vs HOW).

## Workflow Routing

Use the workflow that best matches intent:

- `development`: bug fixes, enhancements, and new features.
- `performance`: bottleneck analysis and optimization.
- `migration`: technology/version/architecture migrations.
- `research`: investigation with documented findings and options.
- `product-design`: feature/product design before implementation.

Use `/flowbit:work` as a unified entrypoint when classification is needed. It auto-routes new tasks and can resume existing task folders.

## Execution Model (Expected Behavior)

For orchestrated workflows:

1. Create or resume a task directory under `.flowbit/tasks/<type>/...`.
2. Gather context and requirements.
3. Produce specification and implementation plan.
4. Execute implementation steps incrementally.
5. Verify correctness (tests, review, readiness checks).
6. Record progress and outcomes in task artifacts.

Every phase should be observable through generated files in the task directory (analysis, implementation, verification, documentation as needed).

## Task Artifact Contract

Typical task structure:

- `orchestrator-state.yml`: current phase, completion status, failure/retry metadata.
- `analysis/`: requirements, discovery notes, and optional research context.
- `implementation/spec.md`: what should be built.
- `implementation/implementation-plan.md`: actionable implementation steps.
- `implementation/work-log.md`: chronological execution log.
- `verification/`: audits, test outcomes, and readiness assessments.
- `documentation/`: optional user-facing docs for completed features.

Do not save workflow artifacts in project root docs or source directories unless explicitly requested.

## Copilot CLI Conventions

- Use single-select prompts sequentially when multiple user decisions are required.
- In this environment, invoke skills via `/flowbit:<skill>` (for example: `/flowbit:init`, `/flowbit:development`).
- Keep `/flowbit:work` as the unified routing command entrypoint.
- Keep command behavior deterministic and tool-driven (avoid vague, free-form execution when a skill/agent exists).
- Prefer plugin-defined skills/agents for delegated tasks rather than re-implementing workflow logic inline.

## Safety and Quality Gates

- Confirm before destructive actions (rollback/reset/remove generated assets).
- Run appropriate verification before concluding implementation.
- Report what was changed, why, and what verification was executed.
- If standards are missing, continue work but recommend initializing Flowbit docs with `/flowbit:init`.

## Command Intent Map

- `/flowbit:work`: classify and route or resume existing workflow tasks.
- `/flowbit:quick-plan`: enter standards-aware planning flow.
- `/flowbit:quick-dev`: implement directly with standards awareness.
- `/flowbit:reviews-*`: delegate to specialized review/audit agents.

Use these entrypoints to keep behavior consistent across sessions and contributors.
