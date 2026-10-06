---
name: quick-plan
description: Use when a user wants an implementation plan before code changes. Read applicable .flowbit/docs/ standards and save the plan under .flowbit/tasks/quick-plan/<slug>/plan.md.
---

# Planning Mode with Standards Awareness

Enter the host's planning mode for a task, with automatic discovery of project standards from `.flowbit/docs/`.

## Usage

```bash
/flowbit:quick-plan [task description]
```

## Examples

```bash
/flowbit:quick-plan "Add user authentication with email/password"
/flowbit:quick-plan "Refactor the payment processing module"
/flowbit:quick-plan
```

---

## Workflow

### Step 1: Parse Input

**Get the task description:**

- If provided as argument, use it directly
- If not provided, use ask_user to prompt:
  ```
  "What would you like to plan? Please describe the task or feature."
  ```

### Step 2: Choose the Task Folder

1. Derive a short lowercase kebab-case `<slug>` from the task description (for example, `add-user-auth`).
2. Use `.flowbit/tasks/quick-plan/<slug>/` as the task folder and `plan.md` as the final plan path.
3. If the folder already exists for the same task, reuse its plan when revising or resuming. For a different task with the same slug, choose the next unused suffix (`<slug>-2`, `<slug>-3`, etc.). Never overwrite an unrelated plan.
4. Create the task folder before entering planning mode. Keep the plan and any supporting task artifacts in this folder, not in the repository root.

### Step 3: Discover and Read Standards (BEFORE Plan Mode)

**CRITICAL: This step MUST complete before calling EnterPlanMode.**

1. **Check if `.flowbit/docs/INDEX.md` exists**
   - **If not exists**: Note that no standards are available, continue to Step 4
   - **If exists**: Continue with discovery below

2. **Read INDEX.md** to understand available standards and documentation

3. **Identify applicable standards** based on:
   - The categories and files listed in INDEX.md
   - The nature of the task being planned
   - Keywords and patterns in the task description (e.g., "API" → api standards, "form" → validation standards, "upload" → file-handling standards)

4. **READ the actual standard files** using the Read tool — reading INDEX.md alone is NOT sufficient

5. **Summarize key guidelines** from each standard file read — these will carry into plan mode as context

### Step 4: Enter Planning Mode

**Enter the host's planning mode (for example, `EnterPlanMode` in Claude Code).**

**Standards context from Step 3 MUST actively inform all plan mode phases:**

- **Explore**: When delegating codebase exploration, include the applicable standard files and key guidelines from Step 3 in the prompt. Verify how the existing codebase follows them.
- **Plan**: Apply these standards to each implementation step, not just in a separate list.
- **Final plan**: Include the standards in the implementation steps and in the mandatory sections below.

The planning mode will:
1. Explore the codebase with standards context; delegate when the scope warrants it
2. Design the implementation approach with standards constraints
3. Review and verify alignment with user intent
4. Write the final plan to `.flowbit/tasks/quick-plan/<slug>/plan.md` (with standards woven into steps). If the host restricts writes to its own plan file during planning mode, validate that file before requesting approval, then copy its final content to the task folder immediately after leaving planning mode and before starting implementation. Do not treat the host's temporary plan file as the saved Flowbit plan.
5. Request user approval through the host's plan-mode exit (gated on mandatory standards sections)

### Plan Approval Gate: Mandatory Standards Sections

**BLOCKING: Do NOT exit planning mode until the final plan contains these sections:**

1. **"## Applicable Standards"** — list each standard file that was read, with key guidelines extracted from each. If no standards exist, state: "No AI SDLC standards found. Consider running `/flowbit:init`."

2. **"## Standards Compliance Checklist"** — checkboxes for each applicable standard guideline that implementation must follow. Example:
   ```markdown
   - [ ] API endpoints follow REST naming conventions (from `standards/backend/api.md`)
   - [ ] Error responses use standard error format (from `standards/backend/api.md`)
   - [ ] New components use TypeScript strict mode (from `standards/frontend/components.md`)
   ```

If these sections are missing, add them before requesting approval. Confirm that `.flowbit/tasks/quick-plan/<slug>/plan.md` contains the approved plan before reporting completion; if writing it fails, report the error and do not claim the plan was saved.

### Graceful Fallback

**If `.flowbit/docs/` does not exist:**

Continue with planning mode normally. The "Applicable Standards" section in the plan should note:

```
No AI SDLC standards found. Consider running `/flowbit:init` to initialize
project documentation and coding standards for better consistency.
```

## What This Does

1. **Parses** task description from user input
2. **Discovers and READS** applicable standard files from `.flowbit/docs/` (BEFORE plan mode)
3. **Enters** the host's planning mode with standards already loaded
4. **Saves** the plan with implementation approach, applicable standards, and compliance checklist to `.flowbit/tasks/quick-plan/<slug>/plan.md`
5. **Gates** plan approval on mandatory standards sections in the plan file

## Benefits Over Manual Planning

- Automatic standards discovery and integration
- Standards read BEFORE planning begins (not as an afterthought)
- Plan saved in a task-specific folder for review before implementation
- Standards compliance checklist built into the plan

## After Planning

Once the plan is approved:
- Report the saved plan path; do not start implementation unless requested
- Standards are applied during coding

## Post-Implementation Verification

After implementation is complete, verify standards compliance using the checklist from the plan:

1. **Review the "Standards Compliance Checklist"** in the plan file
2. **For each checklist item**: verify implementation follows the guideline
3. **Document verification results** (pass/fail for each item)
4. **Address any violations** before marking task complete

This ensures the discovered standards are actually enforced, not just documented.
