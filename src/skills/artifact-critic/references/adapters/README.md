# Adapter Registry

Each adapter extends the same core critique workflow with artifact-specific checks.

Available adapters:
- `diagram.md`
- `plan.md`
- `workflow.md`
- `architecture.md`
- `idea.md`
- `skill-definition.md`
- `review-report.md`
- `other.md`

## Adapter contract

Each adapter should provide:
1. Focus questions (what to challenge first),
2. Failure patterns (common weaknesses),
3. Additional acceptance conditions (artifact-specific).

When adding a new artifact type, create a new adapter file and register it here.
