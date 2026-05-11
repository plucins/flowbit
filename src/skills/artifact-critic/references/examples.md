# Usage Examples

## Plan critique (quick)

Input:

```yaml
artifactType: plan
artifactPath: ".flowbit/tasks/development/2026-05-10-auth/implementation/implementation-plan.md"
intent: "Deliver auth feature with predictable scope"
decisionGate: before_execute
constraints:
  - "2 developer-days"
qualityBar:
  - "No regressions in login flow"
mode: quick
```

## Architecture critique (deep)

Input:

```yaml
artifactType: architecture
artifactInline: "Proposed event-driven redesign for order pipeline with async retries"
intent: "Improve resilience under peak traffic"
decisionGate: before_user_approval
constraints:
  - "No new managed services in this quarter"
qualityBar:
  - "99.9% successful order processing"
evidence:
  - "Current incident report from Q1"
mode: deep
```

## Diagram critique (quick)

Input:

```yaml
artifactType: diagram
artifactPath: "docs/diagrams/dependency-map-overview.mmd"
intent: "Validate dependency boundaries"
decisionGate: ad-hoc
mode: quick
```
