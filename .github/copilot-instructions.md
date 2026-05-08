# Flowbit Copilot CLI Instructions

This repository is a GitHub Copilot CLI plugin that provides SDLC workflows.

## Core expectations

- Use `.flowbit/docs/INDEX.md` as the first project documentation checkpoint when available.
- Never rollback or revert user code without explicit user confirmation.
- Prefer phased execution: analyze -> specify -> plan -> implement -> verify.
- Keep all workflow artifacts under `.flowbit/tasks/<workflow-type>/<task-name>/`.

## Workflow routing

- `development`: bug fixes, enhancements, and new capabilities
- `performance`: optimization and bottlenecks
- `migration`: tech, platform, or version moves
- `research`: investigation and documented findings
- `product-design`: feature/product design before implementation

## Copilot CLI compatibility

- Use single-select prompts when multiple decisions are needed (ask sequentially).
- Use plugin skill names as exposed by Copilot CLI command routing.
- Keep plugin metadata in root `plugin.json`.
