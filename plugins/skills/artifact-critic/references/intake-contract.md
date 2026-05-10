# Intake Contract

Collect this minimal input before critique:

```yaml
artifactType: diagram | plan | workflow | architecture | idea | skill-definition | review-report | other
artifactPath: "<optional file path>"
artifactInline: "<optional inline artifact summary>"
intent: "<what this artifact should achieve>"
decisionGate: before_user_approval | before_execute | before_finalize | ad-hoc
constraints:
  - "<time/budget/team/compliance constraints>"
qualityBar:
  - "<reliability/performance/security/maintainability expectations>"
evidence:
  - "<tests/data/benchmarks/reports that support the artifact>"
mode: quick | deep
```

## Resolution rules

1. If `artifactPath` exists, use it as primary source.
2. If `artifactPath` is missing, require `artifactInline`.
3. If both are missing, stop and ask for a minimal artifact description.
4. If `artifactType` is unknown, set `artifactType=other` and use generic adapter.
5. If `mode` is omitted, default to `quick`.

## Minimum required information

Never proceed without at least:
- artifact content (`artifactPath` or `artifactInline`),
- intended outcome (`intent`),
- decision context (`decisionGate`).
