# Workflows

Ten dokument pokazuje workflow jako osobne diagramy oraz opisuje zaleznosci miedzy nimi.

## Nawigacja

- [Work routing](#work-routing)
- [Init + standards](#init-standards)
- [Development](#development)
- [Performance](#performance)
- [Migration](#migration)
- [Research](#research)
- [Product design](#product-design)
- [Shared internals: codebase-analyzer](#shared-codebase-analyzer)
- [Shared internals: implementation executor](#shared-implementation-executor)
- [Shared internals: implementation verifier](#shared-implementation-verifier)

<a id="work-routing"></a>
## Work routing

Opis:
- Wejscie przez komende `/work` trafia do `task-classifier`.
- `task-classifier` wybiera jedna z 5 sciezek: [Development](#development), [Performance](#performance), [Migration](#migration), [Research](#research), [Product design](#product-design).
- Opcjonalnie `diagrams-mermaid` moze wygenerowac mape routingu/resume dla biezacego zadania.
- Ten diagram jest punktem startowym calego flow.

```mermaid
graph TD
  WORK["⚡ /work"] -- "komenda: /work -> routing" --> CLASS["🤖 task-classifier"]
  CLASS -- "decyzja: typ zadania" --> DEV["🧠 development"]
  CLASS -- "decyzja: typ zadania" --> PERF["🧠 performance"]
  CLASS -- "decyzja: typ zadania" --> MIG["🧠 migration"]
  CLASS -- "decyzja: typ zadania" --> RES["🧠 research"]
  CLASS -- "decyzja: typ zadania" --> PD["🧠 product-design"]
  CLASS -- "opcjonalnie: visual routing map" --> DIAG_W["🧠 diagrams-mermaid"]

  classDef cmd fill:#E8F1FF,stroke:#2563EB,stroke-width:2px,color:#0B3A8F;
  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class WORK cmd;
  class DEV,PERF,MIG,RES,PD,DIAG_W skill;
  class CLASS agent;
```

<a id="init-standards"></a>
## Init + standards

Opis:
- Komenda `/flowbit:init` uruchamia `init skill`.
- `init skill` odpala analize projektu i odkrywanie standardow.
- `init skill` moze wywolac `diagrams-mermaid`, aby uszczegolowic dokumenty `architecture` i `tech-stack` bez zastapienia opisu.
- `docs-operator` agreguje standardy i aktualizuje [docs-manager](#shared-codebase-analyzer) (powiazanie przez wspolne wewnetrzne capability opisu i raportowania).
- Komenda `/flowbit:standards-update` pozwala dograc standardy poza init.

```mermaid
graph TD
  INIT_CMD["⚡ /flowbit:init"] -- "komenda: bootstrap" --> INIT["🧠 init skill"]
  INIT -- "etap: discovery" --> PROJ["🤖 project-analyzer"]
  INIT -- "etap: standards discovery" --> STD_DISC["🧠 standards-discover skill"]
  INIT -- "etap: visual refinement (opcjonalnie)" --> DIAG_I["🧠 diagrams-mermaid"]
  INIT -- "etap: docs sync" --> DOCS_OP["🤖 docs-operator"]
  STD_DISC -- "wejscie: zebrane standardy" --> DOCS_OP
  STD_UPD["⚡ /flowbit:standards-update"] -- "komenda: update standards" --> DOCS_OP
  DIAG_I -- "uzupelnia architecture/tech-stack" --> DOCS_OP
  DOCS_OP -- "etap: utrwalenie dokumentacji" --> DOCS["🧠 docs-manager (preloaded)"]

  classDef cmd fill:#E8F1FF,stroke:#2563EB,stroke-width:2px,color:#0B3A8F;
  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class INIT_CMD,STD_UPD cmd;
  class INIT,STD_DISC,DOCS,DIAG_I skill;
  class PROJ,DOCS_OP agent;
```

<a id="development"></a>
## Development

Opis:
- Sciezka aktywowana przez [Work routing](#work-routing), gdy klasyfikator zwroci `development`.
- Wykorzystuje wspolne komponenty z [Shared internals: codebase-analyzer](#shared-codebase-analyzer), [Shared internals: implementation executor](#shared-implementation-executor) i [Shared internals: implementation verifier](#shared-implementation-verifier).
- Moze uszczegolawiac `spec.md` i `implementation-plan.md` przez `diagrams-mermaid` (diagramy jako dopelnienie tekstu).
- Opcjonalne galezie (`ui-mockup`, `e2e-test-verifier`, `user-docs-generator`) sa zalezne od typu zmian i kryteriow akceptacji.

```mermaid
graph TD
  DEV2["🧠 development"] -- "faza: analiza kodu" --> CODEBASE_D["🧠 codebase-analyzer"]
  DEV2 -- "faza: analiza brakow" --> GAP_D["🤖 gap-analyzer"]
  DEV2 -- "faza: specyfikacja" --> SPEC_D["🤖 specification-creator"]
  DEV2 -- "faza: uszczegolowienie diagramami" --> DIAG_D["🧠 diagrams-mermaid"]
  DEV2 -- "faza: quality gate (warunkowo)" --> AUD_D["🤖 spec-auditor (conditional)"]
  DEV2 -- "faza: plan implementacji" --> PLAN_D["🤖 implementation-planner"]
  DEV2 -- "faza: wykonanie planu" --> EXEC_D["🧠 implementation-plan-executor"]
  DEV2 -- "faza: weryfikacja" --> VER_D["🧠 implementation-verifier"]
  DEV2 -- "faza: UI (jesli ui_heavy)" --> UI_D["🤖 ui-mockup (if ui_heavy)"]
  DEV2 -- "faza: testy E2E (opcjonalnie)" --> E2E_D["🤖 e2e-test-verifier (optional)"]
  DEV2 -- "faza: docs user-facing (opcjonalnie)" --> UDOCS_D["🤖 user-docs-generator (optional)"]

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class DEV2,CODEBASE_D,EXEC_D,VER_D,DIAG_D skill;
  class GAP_D,SPEC_D,AUD_D,PLAN_D,UI_D,E2E_D,UDOCS_D agent;
```

<a id="performance"></a>
## Performance

Opis:
- Sciezka aktywowana przez [Work routing](#work-routing), gdy klasyfikator zwroci `performance`.
- Rozszerza flow o `bottleneck-analyzer`, ale dalej korzysta z [Shared internals: implementation executor](#shared-implementation-executor) i [Shared internals: implementation verifier](#shared-implementation-verifier).
- Moze dodac warstwe wizualna planu/spec przez `diagrams-mermaid`.

```mermaid
graph TD
  PERF2["🧠 performance"] -- "faza: analiza kodu" --> CODEBASE_P["🧠 codebase-analyzer"]
  PERF2 -- "faza: analiza bottleneckow" --> BOT_P["🤖 bottleneck-analyzer"]
  PERF2 -- "faza: specyfikacja optymalizacji" --> SPEC_P["🤖 specification-creator"]
  PERF2 -- "faza: uszczegolowienie diagramami" --> DIAG_P["🧠 diagrams-mermaid"]
  PERF2 -- "faza: quality gate (warunkowo)" --> AUD_P["🤖 spec-auditor (conditional)"]
  PERF2 -- "faza: plan implementacji" --> PLAN_P["🤖 implementation-planner"]
  PERF2 -- "faza: wykonanie planu" --> EXEC_P["🧠 implementation-plan-executor"]
  PERF2 -- "faza: weryfikacja efektu" --> VER_P["🧠 implementation-verifier"]

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class PERF2,CODEBASE_P,EXEC_P,VER_P,DIAG_P skill;
  class BOT_P,SPEC_P,AUD_P,PLAN_P agent;
```

<a id="migration"></a>
## Migration

Opis:
- Sciezka aktywowana przez [Work routing](#work-routing), gdy klasyfikator zwroci `migration`.
- Podobna do [Development](#development), ale nacisk jest na domkniecie migracji i ewentualny output dokumentacyjny dla usera.
- Moze uszczegolowic plan migracji przez `diagrams-mermaid`.

```mermaid
graph TD
  MIG2["🧠 migration"] -- "faza: analiza kodu" --> CODEBASE_M["🧠 codebase-analyzer"]
  MIG2 -- "faza: analiza brakow" --> GAP_M["🤖 gap-analyzer"]
  MIG2 -- "faza: specyfikacja migracji" --> SPEC_M["🤖 specification-creator"]
  MIG2 -- "faza: uszczegolowienie diagramami" --> DIAG_M["🧠 diagrams-mermaid"]
  MIG2 -- "faza: plan implementacji" --> PLAN_M["🤖 implementation-planner"]
  MIG2 -- "faza: wykonanie planu" --> EXEC_M["🧠 implementation-plan-executor"]
  MIG2 -- "faza: weryfikacja migracji" --> VER_M["🧠 implementation-verifier"]
  MIG2 -- "faza: docs user-facing (opcjonalnie)" --> UDOCS_M["🤖 user-docs-generator (optional)"]

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class MIG2,CODEBASE_M,EXEC_M,VER_M,DIAG_M skill;
  class GAP_M,SPEC_M,PLAN_M,UDOCS_M agent;
```

<a id="research"></a>
## Research

Opis:
- Sciezka aktywowana przez [Work routing](#work-routing), gdy klasyfikator zwroci `research`.
- To flow badawcze, ktore moze zasilic [Product design](#product-design) lub [Development](#development) wynikami.
- W fazie design moze wywolac `diagrams-mermaid`, zeby doprecyzowac `high-level-design.md`.

```mermaid
graph TD
  RES2["🧠 research"] -- "faza: plan badania" --> RPLAN["🤖 research-planner"]
  RES2 -- "faza: zbieranie danych (rownolegle)" --> GATHER_R["🤖 information-gatherer (parallel)"]
  RES2 -- "faza: synteza" --> SYN_R["🤖 research-synthesizer"]
  RES2 -- "faza: ideacja (opcjonalnie)" --> BRAIN_R["🤖 solution-brainstormer (optional)"]
  RES2 -- "faza: projekt rozwiazania (opcjonalnie)" --> DESIGN_R["🤖 solution-designer (optional)"]
  RES2 -- "faza: uszczegolowienie diagramami" --> DIAG_R["🧠 diagrams-mermaid"]

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class RES2,DIAG_R skill;
  class RPLAN,GATHER_R,SYN_R,BRAIN_R,DESIGN_R agent;
```

<a id="product-design"></a>
## Product design

Opis:
- Sciezka aktywowana przez [Work routing](#work-routing), gdy klasyfikator zwroci `product-design`.
- Lacznik miedzy mini-research a konceptem UI; moze konsumowac wyniki z [Research](#research).
- Korzysta tez z elementow analizy kodu jak w [Shared internals: codebase-analyzer](#shared-codebase-analyzer).

```mermaid
graph TD
  PD2["🧠 product-design"] -- "faza: analiza kodu (enhancement)" --> CODEBASE_PD["🧠 codebase-analyzer (enhancement)"]
  PD2 -- "faza: mini-research" --> GATHER_PD["🤖 information-gatherer (mini-research)"]
  PD2 -- "faza: ideacja rozwiazania" --> BRAIN_PD["🤖 solution-brainstormer"]
  PD2 -- "faza: fallback UI-focused" --> UI_PD["🤖 ui-mockup-generator (UI-focused fallback)"]

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class PD2,CODEBASE_PD skill;
  class GATHER_PD,BRAIN_PD,UI_PD agent;
```

<a id="shared-codebase-analyzer"></a>
## Shared internals: codebase-analyzer

Opis:
- Ten wewnetrzny flow jest wspolny dla [Development](#development), [Performance](#performance), [Migration](#migration) i [Product design](#product-design).
- `Explore agents` realizuja etap eksploracji kodu, a `codebase-analysis-reporter` domyka raport.

```mermaid
graph TD
  CODEBASE_S["🧠 codebase-analyzer"] -- "faza: eksploracja (parallel)" --> EXPLORE["🤖 Explore agents (parallel)"]
  CODEBASE_S -- "faza: raport" --> REPORT["🤖 codebase-analysis-reporter"]

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class CODEBASE_S skill;
  class EXPLORE,REPORT agent;
```

<a id="shared-implementation-executor"></a>
## Shared internals: implementation executor

Opis:
- Ten fragment jest wspolny dla flow wykonawczych: [Development](#development), [Performance](#performance), [Migration](#migration).
- `implementation-plan-executor` deleguje wykonanie do `task-group-implementer`.

```mermaid
graph TD
  EXEC_S["🧠 implementation-plan-executor"] -- "faza: realizacja task groups" --> TG["🤖 task-group-implementer"]

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class EXEC_S skill;
  class TG agent;
```

<a id="shared-implementation-verifier"></a>
## Shared internals: implementation verifier

Opis:
- Ten fragment jest wspolny dla flow wykonawczych: [Development](#development), [Performance](#performance), [Migration](#migration).
- `implementation-verifier` uruchamia wiele quality gates i raportuje gotowosc produkcyjna.

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
