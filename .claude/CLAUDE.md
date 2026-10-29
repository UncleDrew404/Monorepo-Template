# CLAUDE.md

## Plannable and Agent Assignment Gate

Use this workflow for feature work, bug fixes, refactors, and other multi-step code changes.

### 1. Plan first

- Use the Plannable skill and CLI before implementation.
- Create or update the plan, then report the active part and relevant files.
- Do not implement code, edit source files, or start the next plan part until the user explicitly says `run-next` or invokes `$plannable run-next`.
- A previous `run-next` authorizes only the active part. Completion of one part does not authorize starting the next part.

### 2. Select an agent after `run-next`

- Read `MASTER_PLAN.md`, then run `plannable run-next` and read only the active part.
- Select the most appropriate agent from `.claude/agents/` before starting implementation:

| Work type | Agent |
|---|---|
| UserPage-style UI or frontend implementation | `frontend-developer` |
| Visual design, responsive behavior, or accessibility review | `ui-ux-designer` |
| Django/API architecture or backend workflows | `backend-architect` |
| Models, migrations, or database design | `database-architect` |
| Root-cause investigation or failing tests | `debugger` |
| Code review and verification | `code-reviewer` |
| Large-plan context analysis or coordination | `context-manager` |

- Give the selected agent a self-contained brief containing the active Plannable part, its outcome, files, constraints, acceptance checks, and stop conditions.
- The selected agent performs the active part. The coordinating agent keeps the plan state, reviews the result, runs verification, records evidence, and reports completion.
- If no existing agent fits the work, stop before implementation and ask the user which agent definition should be used or created.
- Do not continue to another Plannable part after verification; wait for another explicit `run-next`.

### 3. Report agent usage

When an agent is generated or assigned, report it in this format:

`Agent Name | Model / Effort | Tasks`

Use one row per agent. If the runtime does not expose a literal effort level, state that explicitly instead of inventing one.
