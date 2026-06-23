# Changelog

All notable changes to this project are documented in this file.

## 1.8.0

- Renamed the `bootstrap` skill to `spec-init`. The skill is now invoked as `/flowbit:spec-init` (previously `/flowbit:bootstrap`). All commands, skills, agents, and documentation references were updated to the new name.

## 1.7.0

- Renamed the `init` skill to `bootstrap`. The skill is now invoked as `/flowbit:bootstrap` (previously `/flowbit:init`). All commands, skills, agents, and documentation references were updated to the new name.

## 1.6.2

- Enforced mandatory user-confirmation gates across all orchestrator skills (`development`, `migration`, `performance`, `product-design`, `research`): every `→ Pause` transition is now explicitly marked as `→ **MANDATORY GATE**` to prevent models from silently bypassing phase checkpoints.
- Added Step 0 (session-reminder conflict resolution) to all orchestrator skills: instructs the model to resolve conflicts between session reminders (e.g., "work without stopping") and workflow gate requirements before execution begins.
- Replaced implicit phase gate notes with explicit `**Phase entry self-check**` blocks at the top of every gated phase, ensuring gates fire even when a prior phase's pause was skipped.
- Updated `orchestrator-framework/references/orchestrator-patterns.md`: added section 2.1 documenting session-reminder conflict resolution rules, and extended the anti-pattern table with entries for gate suppression via session context and auto-acknowledgement via compaction summaries.

## 1.6

- Added new plugin skill `html-renderer` for converting markdown plans, ideas, RFCs, and design notes into self-contained, share-ready HTML files using a warm editorial visual system (opt-in, no auto-trigger).
- Bundled reference assets under `src/skills/html-renderer/references/`: `index.html` (canonical component reference), `styles.css` (full `hr-*` class system inlined into outputs), and `tokens.json` (design tokens).
- Added Mermaid diagram support to html-renderer: `<div class="hr-diagram"><div class="mermaid">…</div></div>` blocks, with the Mermaid CDN script + initializer injected only when at least one ` ```mermaid ` fence is present in the source.
- Added reference diagram examples for flowchart, sequence diagram, state diagram, and entity-relationship diagram.
- Reordered `docs/flow/overview.md` so "Init and standards flow" appears earlier, immediately after the high-level command map.

## 1.5

- Added new plugin skill `artifact-critic` for manual, generic critique of decision artifacts (for example: plans, workflows, architectures, diagrams, ideas, skill definitions, and review reports), with modular adapters for easy future extension.
- Clarified `src/skills/diagrams-mermaid/SKILL.md` to explicitly require Mermaid as the only supported diagram framework.

## 1.4

- Added new incident response command and orchestrator: `/flowbit:incident` + `flowbit-incident`.
- Added incident helper skills: `incident-intake`, `incident-triage`, `incident-evidence`, and `incident-postmortem`.
- Added incident specialist agents: `incident-triage`, `timeline-correlator`, `blast-radius-analyzer`, `mitigation-selector`, and `postmortem-author`.
- Added mandatory code-fix control gate in incident flow: `implementation-planner` -> explicit `ask_user` approval -> `implementation-plan-executor`.
- Added incident task artifact structure under `.flowbit/tasks/incidents/` with evidence, mitigation, verification, and postmortem outputs.
- Updated plugin and flow documentation to include incident workflow mapping.

## 1.3

- Added new skill `flowbit-diagrams-mermaid` for Mermaid-based visualization of planning flows, component communication, and architecture views with adaptive detail selection (including C4 levels).
- Added diagram quality guardrails and anti-pattern handling, including a strict rule to avoid inventing missing domain context.
- Added rule that diagram generation refines existing content and does not replace authoritative document narrative.
- Integrated `flowbit-diagrams-mermaid` into workflow skills:
  - `init` (architecture and tech stack document refinement),
  - `research` (high-level design refinement),
  - `development`, `performance`, `migration` (specification and implementation plan refinement),
  - `work` (optional routing/resume visualization when explicitly requested).
- Updated init reference templates to include dedicated sections for visual architecture and stack flow.
- Updated flow documentation under `docs/flow/` to reflect the new skill invocation paths and optional visualization stages.
