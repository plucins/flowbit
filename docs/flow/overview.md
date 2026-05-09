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
graph TD
  WORK["/work"]
  INIT["/flowbit:init"]
  DEV["/flowbit:development"]
  PERF["/flowbit:performance"]
  MIG["/flowbit:migration"]
  RES["/flowbit:research"]
  PD["/flowbit:product-design"]
  STD_DISC["/flowbit:standards-discover"]
  STD_UPD["/flowbit:standards-update"]
  REV["/flowbit:reviews-*"]
  QBUG["/flowbit:quick-bugfix"]
```

<a id="work-routing"></a>
## Work routing

Opis:
- Sciezka dotyczy tylko komendy `/work`.
- `task-classifier` decyduje, ktory orchestrator uruchomic.
- Kazdy wynik klasyfikacji prowadzi do jednego z flow: `development`, `performance`, `migration`, `research`, `product-design`.
- Dalsze zaleznosci tych flow sa opisane w [Delivery orchestrators and shared skills](#delivery-orchestrators-and-shared-skills) oraz [Orchestrators to specialized agent families](#orchestrators-to-specialized-agent-families).

```mermaid
graph TD
  WORK["/work"] -- "komenda: /work" --> CLASS["task-classifier"]
  CLASS -- "decyzja: dev task" --> S_DEV["development"]
  CLASS -- "decyzja: perf task" --> S_PERF["performance"]
  CLASS -- "decyzja: migration task" --> S_MIG["migration"]
  CLASS -- "decyzja: research task" --> S_RES["research"]
  CLASS -- "decyzja: product-design task" --> S_PD["product-design"]
```

<a id="direct-command-to-orchestrator-mapping"></a>
## Direct command to orchestrator mapping

Opis:
- Te komendy omijaja klasyfikator i uruchamiaja orchestratory bezposrednio.
- `/flowbit:quick-bugfix` uruchamia `quick-bugfix` jako osobny, skrocony flow naprawczy.
- Zaleznosci `init` i standardow sa rozszerzone w [Init and standards flow](#init-and-standards-flow).

```mermaid
graph TD
  INIT["/flowbit:init"] -- "komenda: direct invoke skill" --> S_INIT["init"]
  DEV["/flowbit:development"] -- "komenda: direct invoke skill" --> S_DEV["development"]
  PERF["/flowbit:performance"] -- "komenda: direct invoke skill" --> S_PERF["performance"]
  MIG["/flowbit:migration"] -- "komenda: direct invoke skill" --> S_MIG["migration"]
  RES["/flowbit:research"] -- "komenda: direct invoke skill" --> S_RES["research"]
  PD["/flowbit:product-design"] -- "komenda: direct invoke skill" --> S_PD["product-design"]
  STD_DISC["/flowbit:standards-discover"] -- "komenda: direct invoke skill" --> S_STD_DISC["standards-discover"]
  STD_UPD["/flowbit:standards-update"] -- "komenda: direct invoke skill" --> S_STD_UPD["standards-update"]
  QBUG["/flowbit:quick-bugfix"] -- "komenda: shortcut flow" --> S_QBUG["quick-bugfix"]
```

<a id="init-and-standards-flow"></a>
## Init and standards flow

Opis:
- `init` uruchamia discovery standardow oraz synchronizacje dokumentacji.
- Zarowno `standards-discover`, jak i `standards-update` koncza w `docs-manager (via docs-operator)`.
- Ten flow spina setup projektu z utrzymaniem standardow i docs.

```mermaid
graph TD
  S_INIT["init"] -- "etap: standards discovery" --> S_STD_DISC["standards-discover"]
  S_INIT -- "etap: docs sync" --> S_DOCS["docs-manager (via docs-operator)"]
  S_STD_DISC -- "etap: docs sync" --> S_DOCS
  S_STD_UPD["standards-update"] -- "etap: docs update" --> S_DOCS
```

<a id="delivery-orchestrators-and-shared-skills"></a>
## Delivery orchestrators and shared skills

Opis:
- `development`, `performance`, `migration` wspoldziela te same trzy capability: `codebase-analyzer`, `implementation-plan-executor`, `implementation-verifier`.
- `product-design` uzywa wspolnego `codebase-analyzer`.
- To jest trzon wspolnych zaleznosci wykonawczych; szczegoly agentowe sa w [Orchestrators to specialized agent families](#orchestrators-to-specialized-agent-families).

```mermaid
graph TD
  S_DEV["development"] -- "faza: analiza kodu" --> S_CODEBASE["codebase-analyzer"]
  S_DEV -- "faza: wykonanie planu" --> S_IMPL_EXEC["implementation-plan-executor"]
  S_DEV -- "faza: weryfikacja" --> S_IMPL_VER["implementation-verifier"]

  S_PERF["performance"] -- "faza: analiza kodu" --> S_CODEBASE
  S_PERF -- "faza: wykonanie planu" --> S_IMPL_EXEC
  S_PERF -- "faza: weryfikacja" --> S_IMPL_VER

  S_MIG["migration"] -- "faza: analiza kodu" --> S_CODEBASE
  S_MIG -- "faza: wykonanie planu" --> S_IMPL_EXEC
  S_MIG -- "faza: weryfikacja" --> S_IMPL_VER

  S_PD["product-design"] -- "faza: analiza kodu (enhancement)" --> S_CODEBASE
```

<a id="orchestrators-to-specialized-agent-families"></a>
## Orchestrators to specialized agent families

Opis:
- `A_SPEC` i `A_PLAN` sa rodzina agentow dla flow wykonawczych (`development`, `performance`, `migration`).
- `A_RESEARCH` obsluguje glowne flow badawcze i wspiera `product-design`.
- `A_QUALITY` jest uruchamiane przez `implementation-verifier` i review command.
- `A_OTHER` grupuje narzedzia pomocnicze (analyzery, UI mockup, user docs itp.).

```mermaid
graph TD
  S_DEV["development"] -- "delegacja: spec creation/audit" --> A_SPEC["specification-creator / spec-auditor"]
  S_PERF["performance"] -- "delegacja: spec creation/audit" --> A_SPEC
  S_MIG["migration"] -- "delegacja: spec creation/audit" --> A_SPEC

  S_DEV -- "delegacja: planning/execution" --> A_PLAN["implementation-planner / task-group-implementer"]
  S_PERF -- "delegacja: planning/execution" --> A_PLAN
  S_MIG -- "delegacja: planning/execution" --> A_PLAN

  S_RES["research"] -- "delegacja: research pipeline" --> A_RESEARCH["research-planner / gatherer / synthesizer / brainstormer / designer"]
  S_PD["product-design"] -- "delegacja: mini-research support" --> A_RESEARCH

  S_IMPL_VER["implementation-verifier"] -- "quality gates" --> A_QUALITY["code-reviewer / pragmatist / readiness / reality / tests"]

  S_INIT["init"] -- "delegacja: helper analyzers" --> A_OTHER["project-analyzer / gap-analyzer / ui-mockup / bottleneck / user-docs"]
  S_DEV -- "delegacja: helper analyzers" --> A_OTHER
  S_PERF -- "delegacja: helper analyzers" --> A_OTHER
  S_MIG -- "delegacja: helper analyzers" --> A_OTHER
```

<a id="reviews-and-quick-bugfix-bindings"></a>
## Reviews and quick bugfix bindings

Opis:
- `/flowbit:reviews-*` uruchamia bezposrednio rodzine quality agentow.
- `/flowbit:quick-bugfix` uruchamia `quick-bugfix` jako szybka sciezka operacyjna.
- Ta sekcja domyka mapowanie entry-pointow z [Entry points](#entry-points).

```mermaid
graph TD
  REV["/flowbit:reviews-*"] -- "komenda: quality/review lane" --> A_QUALITY["code-reviewer / pragmatist / readiness / reality / tests"]
  QBUG["/flowbit:quick-bugfix"] -- "komenda: shortcut to fix lane" --> S_QBUG["quick-bugfix"]
```
