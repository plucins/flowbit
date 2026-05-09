# Changelog

All notable changes to this project are documented in this file.

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
