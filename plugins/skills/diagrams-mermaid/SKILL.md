---
name: diagrams-mermaid
description: Creates Mermaid diagrams for planning flows, component communication, and architecture views with adaptive detail selection (including C4 levels). Use when the user asks for a workflow, interaction, or architecture visualization from provided task context.
argument-hint: "[diagram intent or task context]"
---

# Mermaid Diagram Builder

Generate Mermaid diagrams from task context. Focus on visual structure and communication flow, not domain invention.

## Core Rule

**This skill refines existing content, it does NOT replace it.**

Use the current document/task artifact as source material and add visual precision on top of it. Keep required narrative sections intact.

**NEVER invent missing domain context.**

If required domain facts are missing (actors, components, boundaries, protocols, decisions), ask targeted clarification questions before drawing the final diagram. Do not present guesses as facts.

---

## When to Use

Use this skill when the user wants:
- a planning flow diagram,
- component communication visualization,
- architecture-level mapping,
- phase/state transitions in a workflow.

Do not use this skill to maintain plugin documentation diagrams under `docs/flow/*`.

---

## Output Contract

When producing a diagram, return:
1. **Diagram Type + Detail Rationale** (1-3 lines)
2. **Mermaid diagram block**
3. **Open Questions** (only if context is incomplete)

If context is incomplete:
- ask concise clarifying questions first,
- or provide a **Draft diagram** clearly marked as provisional with explicit unknowns.

---

## Workflow

### Step 1: Extract Context

Extract only explicit facts from user input and referenced artifacts:
- scope and objective,
- participants/components,
- boundaries (system/service/module),
- key interactions or state transitions,
- constraints and non-goals.

If any required field is missing for the chosen diagram type, ask for it.

### Step 2: Infer Diagram Goal

Classify what the user needs:
- **Process logic** -> `flowchart`
- **Time-ordered communication** -> `sequenceDiagram`
- **State transitions / phase gates** -> `stateDiagram-v2`
- **Architecture landscape** -> C4 (`C4Context` / `C4Container` / `C4Component`)

### Step 3: Infer Detail Level

Choose the minimum sufficient detail:
- **System-level**: external actors/systems only -> `C4Context`
- **Service/container-level**: apps/services/datastores + interfaces -> `C4Container`
- **Module/component-level**: internal components in one container -> `C4Component` (only when implementation-level planning needs it)

Fallback rule:
- If C4 is too ambiguous or unstable for the requested artifact, use `flowchart` or `sequenceDiagram`.

### Step 4: Build Diagram

Apply naming and structure rules:
- Stable IDs, human-readable labels.
- One concern per diagram (split large scope into multiple diagrams).
- For branching logic, use explicit decisions.
- For interactions, use `alt`, `opt`, `par` where needed.

### Step 5: Validate Quality

Run this checklist before final output:
- Diagram type matches user intent.
- Detail level matches planning need (not over/under detailed).
- No speculative domain entities or links.
- Labels are clear and consistent.
- Syntax is parser-safe.

---

## Mermaid/C4 Best Practices

1. **Goal-first, syntax-second**: pick the view before drawing nodes.
2. **Small readable views**: split diagrams instead of creating one overloaded graph.
3. **Consistent naming**: keep IDs stable and semantic.
4. **Explicit branching/parallelism**: model alternatives and parallel flows clearly.
5. **C4 discipline**: do not mix levels (context/container/component) in one diagram.
6. **Parser safety**:
   - quote labels with special characters,
   - avoid reserved or fragile node IDs,
   - avoid custom style/color directives unless explicitly required.

---

## Anti-Examples (What Not To Do)

1. **Invented domain context (critical anti-pattern)**  
   Adding components, protocols, or dependencies not present in provided context.

2. **Mixed abstraction levels**  
   Combining system landscape and low-level function internals in a single diagram.

3. **Unreadable mega-diagram**  
   Packing too many nodes/edges into one chart without decomposition.

4. **Sequence without temporal semantics**  
   Using `sequenceDiagram` while omitting real call order, branches, or optional paths.

5. **Placeholder naming**  
   IDs and labels like `A`, `B`, `step1` with no domain meaning.

When any anti-pattern appears, stop and refactor the output before returning it.

---

## C4 Detail Decision Matrix

| Need | Recommended Type | Minimum Inputs |
|------|-------------------|----------------|
| Who interacts with the system? | `C4Context` | actors, external systems, system boundary |
| How services/data stores communicate? | `C4Container` | containers, interfaces, protocols/data flow |
| How one service/module is internally organized? | `C4Component` | container scope, components, internal contracts |

If minimum inputs are missing, ask questions instead of guessing.

---

## Integration Targets

Primary usage points:
- `plugins/skills/init/SKILL.md` (init result flow),
- `plugins/skills/init/references/architecture-template.md` (generated architecture output),
- `plugins/skills/work/SKILL.md` (routing/resume decision flows),
- `plugins/skills/orchestrator-framework/references/orchestrator-patterns.md` (phase gate protocol visuals),
- long orchestrator outputs under task artifacts.

Explicitly out of scope for this skill:
- plugin documentation maintenance in `docs/flow/overview.md` and `docs/flow/workflows.md`.

---

## Additional Reference

For templates, anti-pattern examples, and quick decision rules, read:
- `reference.md`
