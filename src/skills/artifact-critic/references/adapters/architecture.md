# Adapter: architecture

## Focus questions

- Does architecture meet reliability and scalability goals?
- Are operational constraints and cost trade-offs explicit?
- Are security boundaries and trust assumptions clear?
- Is migration path feasible from current state?

## Common failure patterns

- Overengineering for current scale.
- Undefined failure modes in critical dependencies.
- Missing observability and incident response hooks.
- Security assumed but not designed.

## Extra acceptance conditions

- Includes operational model (monitoring, alerting, runbook direction).
- Includes security posture assumptions.
- Includes cost and migration implications.
