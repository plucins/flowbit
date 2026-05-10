# Output Contract

Use exactly one of two formats.

## Quick Mode (`mode=quick`)

Return 5-8 bullets:
1. Problem/proposal summary
2. Top assumptions
3. Top risks
4. Best alternative
5. Recommendation
6. Acceptance conditions
7. Contingency plan
8. Open questions (optional if needed)

## Deep Mode (`mode=deep`)

Use this structure:

```markdown
## Artifact Review

### 1) Problem and proposal
- Problem:
- Proposal:
- Decision gate:

### 2) Assumptions
- [A1] ...
  - Validation:
  - If false:

### 3) Top risks
- [R1] ...
  - Impact:
  - Likelihood:
  - Mitigation:

### 4) Alternatives
- [O1] ...
  - Pros:
  - Cons:
- [O2] ...
  - Pros:
  - Cons:

### 5) Trade-off matrix
- Complexity:
- Delivery speed:
- Reliability:
- Scalability:
- Operability:
- Maintenance cost:

### 6) Recommendation
- Choice:
- Why now:
- Acceptance conditions:
- Contingency or rollback:

### 7) Open questions
- ...
```
