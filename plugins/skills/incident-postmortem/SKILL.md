---
name: incident-postmortem
description: Produce blameless postmortem and actionable follow-up backlog from incident artifacts.
user-invocable: false
---

# Incident Postmortem Skill

Generate closure artifacts after mitigation and verification are complete.

## Input parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `task_path` | Yes | Absolute path to incident task directory |
| `verification_status` | Yes | Stabilization verdict from verification phase |

## Execution workflow

### Step 1: Load incident artifacts

Read:

- `{task_path}/analysis/incident-brief.md`
- `{task_path}/analysis/timeline.md`
- `{task_path}/analysis/hypotheses.md`
- `{task_path}/implementation/execution-log.md`
- `{task_path}/verification/incident-verification.md`

### Step 2: Delegate postmortem drafting

Call Task tool:

- `flowbit-postmortem-author`

Subagent output must include:

- root cause statement,
- contributing factors,
- what worked / what failed in response,
- preventive actions.

### Step 3: Write artifacts

Create:

- `{task_path}/documentation/postmortem.md`
- `{task_path}/documentation/followups.md`

### Step 4: Return structured output

```yaml
status: success|partial|failed
postmortem_path: documentation/postmortem.md
followups_path: documentation/followups.md
followup_count: 0
```

## Quality rule

Use blameless language. Follow-ups must be concrete, owner-ready tasks with acceptance signals.
