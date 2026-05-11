# Quality Gates

Run this self-check before finalizing the critique.

## Mandatory checks

- At least 2 realistic alternatives are included.
- Risks include both impact and likelihood.
- Recommendation includes explicit acceptance conditions.
- Recommendation includes contingency or rollback.
- Assumptions include concrete validation methods.
- Missing data is called out explicitly (no fabricated certainty).

## Decision-status tag

End the review with one status:
- `accept` - current artifact is acceptable with listed conditions.
- `revise` - significant issues found; revisions recommended.
- `defer` - insufficient evidence to decide safely.

## Severity guidance

- `High`: can cause major failure, rework, or unsafe release.
- `Medium`: weakens quality, increases delivery or support risk.
- `Low`: improvement opportunity with limited downside.
