---
name: incident-intake
description: Normalize incident reports into a structured brief with severity, impact, and immediate context.
user-invocable: false
---

# Incident Intake Skill

Convert raw incident input into a consistent incident brief before triage starts.

## Input parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `task_path` | Yes | Absolute path to incident task directory |
| `incident_description` | Yes | User-provided incident summary |

## Execution workflow

### Step 1: Build normalized incident brief

Extract and structure:

- symptom summary,
- affected surface (service, endpoint, region, tenant),
- first-seen timestamp (if known),
- business impact,
- known recent changes/deploys.

If required fields are missing, use `ask_user` to fill only critical gaps.

### Step 2: Assess initial severity

Classify tentative severity:

- `SEV-1`: major outage / critical business impact,
- `SEV-2`: significant degradation,
- `SEV-3`: contained issue with limited blast radius.

### Step 3: Write artifacts

Create:

- `{task_path}/analysis/incident-brief.md`
- `{task_path}/analysis/impact-assessment.md`

### Step 4: Return structured output

```yaml
status: success|partial|failed
severity: SEV-1|SEV-2|SEV-3
impact_summary: "..."
brief_path: analysis/incident-brief.md
impact_path: analysis/impact-assessment.md
```

## Output requirements

`incident-brief.md` must include:

1. Incident title
2. Symptoms
3. Scope and impact
4. Known context and assumptions
5. Unknowns to resolve in triage
