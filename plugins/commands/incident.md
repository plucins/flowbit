---
name: incident
description: Incident response entrypoint for production issues, mitigation, and postmortem follow-up.
---

# Incident Response

Use this command when production behavior is degraded, unavailable, or risky enough to require incident-style handling.

## Usage

```bash
/flowbit:incident [incident description]
```

## Examples

```bash
/flowbit:incident "SEV-1: Checkout API returns 500 for EU region"
/flowbit:incident "Latency spike after deploy in billing worker"
/flowbit:incident "Intermittent auth timeouts for mobile users"
```

## Workflow

1. Parse incident context (symptoms, impact, timeline hints, known changes).
2. Invoke orchestrator skill `flowbit-incident`.
3. The orchestrator:
   - triages severity and blast radius,
   - gathers evidence and correlates hypotheses,
   - proposes mitigation options,
   - optionally routes code-fix work through implementation planning with explicit user approval before execution,
   - verifies stabilization and produces a postmortem + follow-up actions.

## Notes

- This command is optimized for active production incidents and urgent service degradation.
- For small, well-scoped code issues without incident process overhead, use `/flowbit:quick-bugfix`.
- For planned feature work, use `/work` or `/flowbit:development`.
