# Overview

Ten dokument rozbija overview na osobne, czytelne diagramy oraz opisuje zaleznosci miedzy sekcjami.

## Nawigacja

- [Entry points](#entry-points)
- [Work routing](#work-routing)
- [Direct command to orchestrator mapping](#direct-command-to-orchestrator-mapping)
- [Init and standards flow](#init-and-standards-flow)
- [Delivery orchestrators and shared skills](#delivery-orchestrators-and-shared-skills)
- [Orchestrators to specialized agent families](#orchestrators-to-specialized-agent-families)
- [Reviews and quick bugfix bindings](#reviews-and-quick-bugfix-bindings)

<a id="entry-points"></a>
## Entry points

Opis:
- To sa wszystkie komendy wejsciowe, ktore odpalaja routing lub orchestratory.
- `/work` przechodzi przez [Work routing](#work-routing).
- Komendy skillowe `/flowbit:*` mapuja sie bezposrednio do orchestratorow w [Direct command to orchestrator mapping](#direct-command-to-orchestrator-mapping).
- Komendy review (`/flowbit:reviews-*`) i quick bugfix (`/flowbit:quick-bugfix`) maja dodatkowe powiazania opisane w [Reviews and quick bugfix bindings](#reviews-and-quick-bugfix-bindings).

```mermaid
graph LR
  subgraph C1[" "]
    direction TB
    WORK["⚡ /work"]
    INIT["⚡ /flowbit:init"]
    PERF["⚡ /flowbit:performance"]
    RES["⚡ /flowbit:research"]
  end

  subgraph C2[" "]
    direction TB
    DEV["⚡ /flowbit:development"]
    MIG["⚡ /flowbit:migration"]
    PD["⚡ /flowbit:product-design"]
  end

  subgraph C3[" "]
    direction TB
    STD_DISC["⚡ /flowbit:standards-discover"]
    STD_UPD["⚡ /flowbit:standards-update"]
    REV["⚡ /flowbit:reviews-*"]
    QBUG["⚡ /flowbit:quick-bugfix"]
  end

  classDef cmd fill:#E8F1FF,stroke:#2563EB,stroke-width:2px,color:#0B3A8F;
  class WORK,INIT,DEV,PERF,MIG,RES,PD,STD_DISC,STD_UPD,REV,QBUG cmd;
```

<a id="work-routing"></a>
## Work routing

Opis:
- Sciezka dotyczy tylko komendy `/work`.
- `task-classifier` decyduje, ktory orchestrator uruchomic.
- Kazdy wynik klasyfikacji prowadzi do jednego z flow: `development`, `performance`, `migration`, `research`, `product-design`.
- Opcjonalnie `diagrams-mermaid` moze zwizualizowac routing/resume na bazie potwierdzonego stanu.
- Dalsze zaleznosci tych flow sa opisane w [Delivery orchestrators and shared skills](#delivery-orchestrators-and-shared-skills) oraz [Orchestrators to specialized agent families](#orchestrators-to-specialized-agent-families).

```mermaid
graph TD
  WORK["⚡ /work"] -- "komenda: /work" --> CLASS["🤖 task-classifier"]
  CLASS -- "decyzja: dev task" --> S_DEV["🧠 development"]
  CLASS -- "decyzja: perf task" --> S_PERF["🧠 performance"]
  CLASS -- "decyzja: migration task" --> S_MIG["🧠 migration"]
  CLASS -- "decyzja: research task" --> S_RES["🧠 research"]
  CLASS -- "decyzja: product-design task" --> S_PD["🧠 product-design"]
  CLASS -- "opcjonalnie: visual routing map" --> S_DIAG_WR["🧠 diagrams-mermaid"]

  classDef cmd fill:#E8F1FF,stroke:#2563EB,stroke-width:2px,color:#0B3A8F;
  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class WORK cmd;
  class S_DEV,S_PERF,S_MIG,S_RES,S_PD,S_DIAG_WR skill;
  class CLASS agent;
```

<a id="direct-command-to-orchestrator-mapping"></a>
## Direct command to orchestrator mapping

Opis:
- Te komendy omijaja klasyfikator i uruchamiaja orchestratory bezposrednio.
- `/flowbit:quick-bugfix` uruchamia `quick-bugfix` jako osobny, skrocony flow naprawczy.
- Zaleznosci `init` i standardow sa rozszerzone w [Init and standards flow](#init-and-standards-flow).

```mermaid
graph TD
  INIT["⚡ /flowbit:init"] -- "komenda: direct invoke skill" --> S_INIT["🧠 init"]
  DEV["⚡ /flowbit:development"] -- "komenda: direct invoke skill" --> S_DEV["🧠 development"]
  PERF["⚡ /flowbit:performance"] -- "komenda: direct invoke skill" --> S_PERF["🧠 performance"]
  MIG["⚡ /flowbit:migration"] -- "komenda: direct invoke skill" --> S_MIG["🧠 migration"]
  RES["⚡ /flowbit:research"] -- "komenda: direct invoke skill" --> S_RES["🧠 research"]
  PD["⚡ /flowbit:product-design"] -- "komenda: direct invoke skill" --> S_PD["🧠 product-design"]
  STD_DISC["⚡ /flowbit:standards-discover"] -- "komenda: direct invoke skill" --> S_STD_DISC["🧠 standards-discover"]
  STD_UPD["⚡ /flowbit:standards-update"] -- "komenda: direct invoke skill" --> S_STD_UPD["🧠 standards-update"]
  QBUG["⚡ /flowbit:quick-bugfix"] -- "komenda: shortcut flow" --> S_QBUG["🧠 quick-bugfix"]

  classDef cmd fill:#E8F1FF,stroke:#2563EB,stroke-width:2px,color:#0B3A8F;
  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  class INIT,DEV,PERF,MIG,RES,PD,STD_DISC,STD_UPD,QBUG cmd;
  class S_INIT,S_DEV,S_PERF,S_MIG,S_RES,S_PD,S_STD_DISC,S_STD_UPD,S_QBUG skill;
```

<a id="init-and-standards-flow"></a>
## Init and standards flow

Opis:
- `init` uruchamia discovery standardow oraz synchronizacje dokumentacji.
- Zarowno `standards-discover`, jak i `standards-update` koncza w `docs-manager (via docs-operator)`.
- `init` moze wywolac `diagrams-mermaid`, zeby uszczegolowic wygenerowane dokumenty (np. architecture/tech-stack) bez zastepowania tresci.
- Ten flow spina setup projektu z utrzymaniem standardow i docs.

```mermaid
graph TD
  S_INIT["🧠 init"] -- "etap: standards discovery" --> S_STD_DISC["🧠 standards-discover"]
  S_INIT -- "etap: docs sync" --> S_DOCS["🧠 docs-manager (via docs-operator)"]
  S_INIT -- "etap: visual refinement (opcjonalnie)" --> S_DIAG["🧠 diagrams-mermaid"]
  S_STD_DISC -- "etap: docs sync" --> S_DOCS
  S_STD_UPD["🧠 standards-update"] -- "etap: docs update" --> S_DOCS
  S_DIAG -- "uzupelnia: architecture/tech-stack docs" --> S_DOCS

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  class S_INIT,S_STD_DISC,S_DOCS,S_STD_UPD,S_DIAG skill;
```

<a id="delivery-orchestrators-and-shared-skills"></a>
## Delivery orchestrators and shared skills

Opis:
- `development`, `performance`, `migration` wspoldziela te same trzy capability: `codebase-analyzer`, `implementation-plan-executor`, `implementation-verifier`.
- Te orchestratory moga wywolywac `diagrams-mermaid` do uszczegolowienia `spec` i `implementation-plan` (diagramy jako dodatek do opisu).
- `product-design` uzywa wspolnego `codebase-analyzer`.
- To jest trzon wspolnych zaleznosci wykonawczych; szczegoly agentowe sa w [Orchestrators to specialized agent families](#orchestrators-to-specialized-agent-families).

```mermaid
graph TD
  S_DEV["🧠 development"] -- "faza: analiza kodu" --> S_CODEBASE["🧠 codebase-analyzer"]
  S_DEV -- "faza: diagram refinement (spec/plan)" --> S_DIAG["🧠 diagrams-mermaid"]
  S_DEV -- "faza: wykonanie planu" --> S_IMPL_EXEC["🧠 implementation-plan-executor"]
  S_DEV -- "faza: weryfikacja" --> S_IMPL_VER["🧠 implementation-verifier"]

  S_PERF["🧠 performance"] -- "faza: analiza kodu" --> S_CODEBASE
  S_PERF -- "faza: diagram refinement (spec/plan)" --> S_DIAG
  S_PERF -- "faza: wykonanie planu" --> S_IMPL_EXEC
  S_PERF -- "faza: weryfikacja" --> S_IMPL_VER

  S_MIG["🧠 migration"] -- "faza: analiza kodu" --> S_CODEBASE
  S_MIG -- "faza: diagram refinement (spec/plan)" --> S_DIAG
  S_MIG -- "faza: wykonanie planu" --> S_IMPL_EXEC
  S_MIG -- "faza: weryfikacja" --> S_IMPL_VER

  S_PD["🧠 product-design"] -- "faza: analiza kodu (enhancement)" --> S_CODEBASE

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  class S_DEV,S_CODEBASE,S_IMPL_EXEC,S_IMPL_VER,S_PERF,S_MIG,S_PD,S_DIAG skill;
```

<a id="orchestrators-to-specialized-agent-families"></a>
## Orchestrators to specialized agent families

Opis:
- `A_SPEC` i `A_PLAN` sa rodzina agentow dla flow wykonawczych (`development`, `performance`, `migration`).
- `A_RESEARCH` obsluguje glowne flow badawcze i wspiera `product-design`.
- `A_QUALITY` jest uruchamiane przez `implementation-verifier` i review command.
- `A_OTHER` grupuje narzedzia pomocnicze (analyzery, UI mockup, user docs itp.).
- `diagrams-mermaid` jest skillowym capability do wizualizacji przeplywow i komunikacji na bazie istniejacego contentu.

```mermaid
graph TD
  S_DEV["🧠 development"] -- "delegacja: spec creation/audit" --> A_SPEC["🤖 specification-creator / spec-auditor"]
  S_PERF["🧠 performance"] -- "delegacja: spec creation/audit" --> A_SPEC
  S_MIG["🧠 migration"] -- "delegacja: spec creation/audit" --> A_SPEC

  S_DEV -- "delegacja: planning/execution" --> A_PLAN["🤖 implementation-planner / task-group-implementer"]
  S_PERF -- "delegacja: planning/execution" --> A_PLAN
  S_MIG -- "delegacja: planning/execution" --> A_PLAN

  S_RES["🧠 research"] -- "delegacja: research pipeline" --> A_RESEARCH["🤖 research-planner / gatherer / synthesizer / brainstormer / designer"]
  S_RES -- "delegacja: visual refinement (design docs)" --> S_DIAG["🧠 diagrams-mermaid"]
  S_PD["🧠 product-design"] -- "delegacja: mini-research support" --> A_RESEARCH

  S_IMPL_VER["🧠 implementation-verifier"] -- "quality gates" --> A_QUALITY["🤖 code-reviewer / pragmatist / readiness / reality / tests"]

  S_INIT["🧠 init"] -- "delegacja: helper analyzers" --> A_OTHER["🤖 project-analyzer / gap-analyzer / ui-mockup / bottleneck / user-docs"]
  S_DEV -- "delegacja: helper analyzers" --> A_OTHER
  S_PERF -- "delegacja: helper analyzers" --> A_OTHER
  S_MIG -- "delegacja: helper analyzers" --> A_OTHER

  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class S_DEV,S_PERF,S_MIG,S_RES,S_PD,S_IMPL_VER,S_INIT skill;
  class A_SPEC,A_PLAN,A_RESEARCH,A_QUALITY,A_OTHER agent;
```

<a id="reviews-and-quick-bugfix-bindings"></a>
## Reviews and quick bugfix bindings

Opis:
- `/flowbit:reviews-*` uruchamia bezposrednio rodzine quality agentow.
- `/flowbit:quick-bugfix` uruchamia `quick-bugfix` jako szybka sciezka operacyjna.
- Ta sekcja domyka mapowanie entry-pointow z [Entry points](#entry-points).

```mermaid
graph TD
  REV["⚡ /flowbit:reviews-*"] -- "komenda: quality/review lane" --> A_QUALITY["🤖 code-reviewer / pragmatist / readiness / reality / tests"]
  QBUG["⚡ /flowbit:quick-bugfix"] -- "komenda: shortcut to fix lane" --> S_QBUG["🧠 quick-bugfix"]

  classDef cmd fill:#E8F1FF,stroke:#2563EB,stroke-width:2px,color:#0B3A8F;
  classDef skill fill:#EAFBF1,stroke:#16A34A,stroke-width:2px,color:#14532D;
  classDef agent fill:#FFF4E8,stroke:#EA580C,stroke-width:2px,color:#7C2D12;
  class REV,QBUG cmd;
  class S_QBUG skill;
  class A_QUALITY agent;
```
