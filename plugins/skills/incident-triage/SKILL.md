---
name: incident-triage
description: Triage incidents and propose immediate containment strategies with explicit risk communication.
user-invocable: false
---

# Incident Triage Skill

Decide whether to contain immediately or continue diagnosis first.

## Input parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `task_path` | Yes | Absolute path to incident task directory |
| `severity` | Yes | Current severity classification |
| `impact_summary` | Yes | High-level impact statement |

## Execution workflow

### Step 1: Gather triage signals

Read:

- `{task_path}/analysis/incident-brief.md`
- `{task_path}/analysis/impact-assessment.md`

Then call Task tool:

- `flowbit-incident-triage`

The subagent must provide:

- blast radius estimate,
- confidence in incident scope,
- recommended containment options ranked by time-to-impact.

### Step 2: Build triage decision

Create `{task_path}/analysis/triage-decision.md` with:

1. Current operating status
2. Recommended containment options (ranked)
3. Risks and trade-offs
4. Reversible vs irreversible actions

### Step 3: Return structured output

```yaml
status: success|partial|failed
blast_radius: "..."
containment_strategy: "..."
decision_path: analysis/triage-decision.md
requires_user_confirmation: true
```

## Safety rule

If rollback is among recommended actions, mark it clearly and require explicit user approval in the parent orchestrator.
