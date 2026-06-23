# Workflows

This document shows each workflow as a separate diagram and describes dependencies between them.

## Navigation

- [Work routing](#work-routing)
- [Init + standards](#init-standards)
- [Development](#development)
- [Performance](#performance)
- [Migration](#migration)
- [Research](#research)
- [Product design](#product-design)
- [Incident](#incident)
- [Shared internals: codebase-analyzer](#shared-codebase-analyzer)
- [Shared internals: implementation executor](#shared-implementation-executor)
- [Shared internals: implementation verifier](#shared-implementation-verifier)

<a id="work-routing"></a>
## Work routing

Description:
- Entry via the `/flowbit:work` command goes to `task-classifier`.
- `task-classifier` selects one of 5 paths: [Development](#development), [Performance](#performance), [Migration](#migration), [Research](#research), [Product design](#product-design).
- The `incident` lane is triggered directly by `/flowbit:incident` (no `/flowbit:work` routing changes at this stage).
- Optionally, `diagrams-mermaid` can generate a routing/resume map for the current task.
- This diagram is the starting point of the entire flow.

```mermaid
graph TD
  WORK["⚡ /flowbit:work"] -- "command: /flowbit:work -> routing" --> CLASS["🤖 task-classifier"]
  CLASS -- "decision: task type" --> DEV["🧠 development"]
  CLASS -- "decision: task type" --> PERF["🧠 performance"]
  CLASS -- "decision: task type" --> MIG["🧠 migration"]
  CLASS -- "decision: task type" --> RES["🧠 research"]
  CLASS -- "decision: task type" --> PD["🧠 product-design"]
  CLASS -- "optional: visual routing map" --> DIAG_W["🧠 diagrams-mermaid"]

  classDef cmd fill:#E8F1FF,stroke:#2563EB,stroke-width:2px,color:#0B3A8F;
  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class WORK cmd;
  class DEV,PERF,MIG,RES,PD,DIAG_W skill;
  class CLASS agent;
```

<a id="init-standards"></a>
## Init + standards

Description:
- The `/flowbit:spec-init` command starts the `spec-init skill`.
- `spec-init skill` triggers project analysis and standards discovery.
- `spec-init skill` can call `diagrams-mermaid` to refine `architecture` and `tech-stack` documents without replacing their descriptions.
- `docs-operator` aggregates standards and updates `docs-manager`.
- The `/flowbit:standards-update` command allows updating standards outside of spec-init.

```mermaid
graph TD
  INIT_CMD["⚡ /flowbit:spec-init"] -- "command: spec-init" --> INIT["🧠 spec-init skill"]
  INIT -- "phase: discovery" --> PROJ["🤖 project-analyzer"]
  INIT -- "phase: standards discovery" --> STD_DISC["🧠 standards-discover skill"]
  INIT -- "phase: visual refinement (optional)" --> DIAG_I["🧠 diagrams-mermaid"]
  INIT -- "phase: docs sync" --> DOCS_OP["🤖 docs-operator"]
  STD_DISC -- "input: collected standards" --> DOCS_OP
  STD_UPD["⚡ /flowbit:standards-update"] -- "command: update standards" --> DOCS_OP
  DIAG_I -- "supplements architecture/tech-stack" --> DOCS_OP
  DOCS_OP -- "phase: documentation persistence" --> DOCS["🧠 docs-manager"]

  classDef cmd fill:#E8F1FF,stroke:#2563EB,stroke-width:2px,color:#0B3A8F;
  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class INIT_CMD,STD_UPD cmd;
  class INIT,STD_DISC,DOCS,DIAG_I skill;
  class PROJ,DOCS_OP agent;
```

<a id="development"></a>
## Development

Description:
- Path activated by [Work routing](#work-routing) when the classifier returns `development`.
- Uses shared components from [Shared internals: codebase-analyzer](#shared-codebase-analyzer), [Shared internals: implementation executor](#shared-implementation-executor) and [Shared internals: implementation verifier](#shared-implementation-verifier).
- Can refine `spec.md` and `implementation-plan.md` via `diagrams-mermaid` (diagrams as a complement to text).
- Optional branches (`ui-mockup`, `e2e-test-verifier`, `user-docs-generator`) depend on the type of changes and acceptance criteria.

```mermaid
graph TD
  DEV2["🧠 development"] -- "phase: code analysis" --> CODEBASE_D["🧠 codebase-analyzer"]
  DEV2 -- "phase: gap analysis" --> GAP_D["🤖 gap-analyzer"]
  DEV2 -- "phase: specification" --> SPEC_D["🤖 specification-creator"]
  DEV2 -- "phase: diagram refinement" --> DIAG_D["🧠 diagrams-mermaid"]
  DEV2 -- "phase: quality gate (conditional)" --> AUD_D["🤖 spec-auditor (conditional)"]
  DEV2 -- "phase: implementation plan" --> PLAN_D["🤖 implementation-planner"]
  DEV2 -- "phase: plan execution" --> EXEC_D["🧠 implementation-plan-executor"]
  DEV2 -- "phase: verification" --> VER_D["🧠 implementation-verifier"]
  DEV2 -- "phase: UI (if ui_heavy)" --> UI_D["🤖 ui-mockup (if ui_heavy)"]
  DEV2 -- "phase: E2E tests (optional)" --> E2E_D["🤖 e2e-test-verifier (optional)"]
  DEV2 -- "phase: user-facing docs (optional)" --> UDOCS_D["🤖 user-docs-generator (optional)"]

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class DEV2,CODEBASE_D,EXEC_D,VER_D,DIAG_D skill;
  class GAP_D,SPEC_D,AUD_D,PLAN_D,UI_D,E2E_D,UDOCS_D agent;
```

<a id="performance"></a>
## Performance

Description:
- Path activated by [Work routing](#work-routing) when the classifier returns `performance`.
- Extends the flow with `bottleneck-analyzer`, but still uses [Shared internals: implementation executor](#shared-implementation-executor) and [Shared internals: implementation verifier](#shared-implementation-verifier).
- Can add a visual layer to the plan/spec via `diagrams-mermaid`.

```mermaid
graph TD
  PERF2["🧠 performance"] -- "phase: code analysis" --> CODEBASE_P["🧠 codebase-analyzer"]
  PERF2 -- "phase: bottleneck analysis" --> BOT_P["🤖 bottleneck-analyzer"]
  PERF2 -- "phase: optimization specification" --> SPEC_P["🤖 specification-creator"]
  PERF2 -- "phase: diagram refinement" --> DIAG_P["🧠 diagrams-mermaid"]
  PERF2 -- "phase: quality gate (conditional)" --> AUD_P["🤖 spec-auditor (conditional)"]
  PERF2 -- "phase: implementation plan" --> PLAN_P["🤖 implementation-planner"]
  PERF2 -- "phase: plan execution" --> EXEC_P["🧠 implementation-plan-executor"]
  PERF2 -- "phase: effect verification" --> VER_P["🧠 implementation-verifier"]

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class PERF2,CODEBASE_P,EXEC_P,VER_P,DIAG_P skill;
  class BOT_P,SPEC_P,AUD_P,PLAN_P agent;
```

<a id="migration"></a>
## Migration

Description:
- Path activated by [Work routing](#work-routing) when the classifier returns `migration`.
- Similar to [Development](#development), but the focus is on completing the migration and optionally producing user-facing documentation.
- Can refine the migration plan via `diagrams-mermaid`.

```mermaid
graph TD
  MIG2["🧠 migration"] -- "phase: code analysis" --> CODEBASE_M["🧠 codebase-analyzer"]
  MIG2 -- "phase: gap analysis" --> GAP_M["🤖 gap-analyzer"]
  MIG2 -- "phase: migration specification" --> SPEC_M["🤖 specification-creator"]
  MIG2 -- "phase: diagram refinement" --> DIAG_M["🧠 diagrams-mermaid"]
  MIG2 -- "phase: implementation plan" --> PLAN_M["🤖 implementation-planner"]
  MIG2 -- "phase: plan execution" --> EXEC_M["🧠 implementation-plan-executor"]
  MIG2 -- "phase: migration verification" --> VER_M["🧠 implementation-verifier"]
  MIG2 -- "phase: user-facing docs (optional)" --> UDOCS_M["🤖 user-docs-generator (optional)"]

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class MIG2,CODEBASE_M,EXEC_M,VER_M,DIAG_M skill;
  class GAP_M,SPEC_M,PLAN_M,UDOCS_M agent;
```

<a id="research"></a>
## Research

Description:
- Path activated by [Work routing](#work-routing) when the classifier returns `research`.
- This is a research flow that can feed [Product design](#product-design) or [Development](#development) with its findings.
- In the design phase, it can call `diagrams-mermaid` to refine `high-level-design.md`.

```mermaid
graph TD
  RES2["🧠 research"] -- "phase: research plan" --> RPLAN["🤖 research-planner"]
  RES2 -- "phase: data gathering (parallel)" --> GATHER_R["🤖 information-gatherer (parallel)"]
  RES2 -- "phase: synthesis" --> SYN_R["🤖 research-synthesizer"]
  RES2 -- "phase: ideation (optional)" --> BRAIN_R["🤖 solution-brainstormer (optional)"]
  RES2 -- "phase: solution design (optional)" --> DESIGN_R["🤖 solution-designer (optional)"]
  RES2 -- "phase: diagram refinement" --> DIAG_R["🧠 diagrams-mermaid"]

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class RES2,DIAG_R skill;
  class RPLAN,GATHER_R,SYN_R,BRAIN_R,DESIGN_R agent;
```

<a id="product-design"></a>
## Product design

Description:
- Path activated by [Work routing](#work-routing) when the classifier returns `product-design`.
- Bridge between mini-research and UI concept; can consume results from [Research](#research).
- Also uses code analysis elements as in [Shared internals: codebase-analyzer](#shared-codebase-analyzer).

```mermaid
graph TD
  PD2["🧠 product-design"] -- "phase: code analysis (enhancement)" --> CODEBASE_PD["🧠 codebase-analyzer (enhancement)"]
  PD2 -- "phase: mini-research" --> GATHER_PD["🤖 information-gatherer (mini-research)"]
  PD2 -- "phase: solution ideation" --> BRAIN_PD["🤖 solution-brainstormer"]
  PD2 -- "phase: UI-focused fallback" --> UI_PD["🤖 ui-mockup-generator (UI-focused fallback)"]

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class PD2,CODEBASE_PD skill;
  class GATHER_PD,BRAIN_PD,UI_PD agent;
```

<a id="incident"></a>
## Incident

Description:
- Path activated directly by the `/flowbit:incident` command.
- This is an operational flow: intake, triage, evidence, mitigation, verification, and postmortem.
- For `code_fix`, it reuses `implementation-planner` and `implementation-plan-executor`.
- A hard `ask_user` gate (approve / revise / stop) is required before running the executor.
- For `operational` mitigation, stabilization uses `reality-assessor` directly. For `code_fix`/`hybrid`, `implementation-verifier` is called.
- `incident-triage` is an agent (not a skill wrapper).

```mermaid
graph TD
  INC2["🧠 incident"] -- "phase: intake + severity" --> INTAKE_I["🧠 incident-intake"]
  INC2 -- "phase: triage + containment" --> TRIAGE_I["🤖 incident-triage"]
  INC2 -- "phase: evidence + timeline" --> EVID_I["🧠 incident-evidence"]
  INC2 -- "phase: root cause + strategy" --> MITSEL_I["🤖 mitigation-selector"]
  INC2 -- "phase: planning (when code_fix)" --> PLAN_I["🤖 implementation-planner (conditional)"]
  PLAN_I -- "gate: ask_user approval (mandatory)" --> EXEC_I["🧠 implementation-plan-executor (conditional)"]
  INC2 -- "phase: stabilization (code_fix/hybrid)" --> VER_I["🧠 implementation-verifier"]
  INC2 -- "phase: stabilization (operational)" --> REAL_I["🤖 reality-assessor (conditional)"]
  INC2 -- "phase: closure + followups" --> POST_I["🧠 incident-postmortem"]
  POST_I -- "delegation: postmortem draft" --> PM_A["🤖 postmortem-author"]

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class INC2,INTAKE_I,EVID_I,EXEC_I,VER_I,POST_I skill;
  class MITSEL_I,PLAN_I,PM_A,TRIAGE_I,REAL_I agent;
```

<a id="shared-codebase-analyzer"></a>
## Shared internals: codebase-analyzer

Description:
- This internal flow is shared by [Development](#development), [Performance](#performance), [Migration](#migration), and [Product design](#product-design).
- `Explore agents` handle the code exploration phase, and `codebase-analysis-reporter` closes the report.

```mermaid
graph TD
  CODEBASE_S["🧠 codebase-analyzer"] -- "phase: exploration (parallel)" --> EXPLORE["🤖 Explore agents (parallel)"]
  CODEBASE_S -- "phase: report" --> REPORT["🤖 codebase-analysis-reporter"]

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class CODEBASE_S skill;
  class EXPLORE,REPORT agent;
```

<a id="shared-implementation-executor"></a>
## Shared internals: implementation executor

Description:
- This fragment is shared by execution flows: [Development](#development), [Performance](#performance), [Migration](#migration).
- In [Incident](#incident) it is triggered conditionally when the mitigation path requires a `code_fix`.
- `implementation-plan-executor` delegates execution to `task-group-implementer`.

```mermaid
graph TD
  EXEC_S["🧠 implementation-plan-executor"] -- "phase: task group execution" --> TG["🤖 task-group-implementer"]

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class EXEC_S skill;
  class TG agent;
```

<a id="shared-implementation-verifier"></a>
## Shared internals: implementation verifier

Description:
- This fragment is shared by execution flows: [Development](#development), [Performance](#performance), [Migration](#migration).
- In [Incident](#incident) it serves as a stabilization quality gate after mitigation execution.
- `implementation-verifier` runs multiple quality gates and reports production readiness.

```mermaid
graph TD
  VER_S["🧠 implementation-verifier"] -- "check: completeness" --> COMP["🤖 implementation-completeness-checker"]
  VER_S -- "check: test suite" --> TEST["🤖 test-suite-runner"]
  VER_S -- "check: code review" --> REVIEW["🤖 code-reviewer"]
  VER_S -- "check: pragmatic quality" --> PRAG["🤖 code-quality-pragmatist"]
  VER_S -- "check: prod readiness" --> PROD["🤖 production-readiness-checker"]
  VER_S -- "check: reality alignment" --> REAL["🤖 reality-assessor"]

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class VER_S skill;
  class COMP,TEST,REVIEW,PRAG,PROD,REAL agent;
```
