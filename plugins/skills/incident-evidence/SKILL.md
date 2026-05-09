---
name: incident-evidence
description: Collect incident evidence and build a timeline for root-cause analysis and mitigation decisions.
user-invocable: false
---

# Incident Evidence Skill

Collect concrete signals first, then correlate timeline and likely fault domains.

## Input parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `task_path` | Yes | Absolute path to incident task directory |
| `incident_description` | Yes | Incident context for evidence targeting |

## Execution workflow

### Step 1: Collect evidence

Call Task tool:

- `flowbit-information-gatherer`

Prompt must focus on incident-relevant sources:

- error logs and traces,
- recent deploy/config changes,
- impacted code paths,
- architecture and runbook docs.

### Step 2: Correlate timeline

Call Task tool:

- `flowbit-timeline-correlator`

Input:

- incident brief,
- gathered findings,
- any deploy/change timestamps.

### Step 3: Write artifacts

Create:

- `{task_path}/analysis/evidence-pack.md`
- `{task_path}/analysis/timeline.md`

### Step 4: Return structured output

```yaml
status: success|partial|failed
evidence_path: analysis/evidence-pack.md
timeline_path: analysis/timeline.md
confidence: high|medium|low
```

## Output requirements

`evidence-pack.md` must separate:

1. confirmed observations,
2. inferred signals,
3. unresolved gaps.
