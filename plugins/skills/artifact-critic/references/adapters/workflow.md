# Adapter: workflow

## Focus questions

- Are phase gates and handoffs explicit?
- Is responsibility clear at each step?
- Are retries, escalation, and stop conditions defined?
- Is observability built in (what indicates success/failure)?

## Common failure patterns

- Implicit transitions between phases.
- Undefined owner for failure handling.
- No timeout/escalation behavior.
- Verification happens too late.

## Extra acceptance conditions

- Handoffs include owner and expected artifact.
- Failure handling has explicit branch rules.
- Verification points exist before irreversible actions.
