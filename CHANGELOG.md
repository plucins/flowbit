# Changelog

All notable changes to this project are documented in this file.

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
