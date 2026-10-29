# Repository Agent Guide

This file is the canonical operating guide for AI agents working in this repository. Read it before changing files. The definitions under `.claude/agents/` and `.claude/skills/` remain the source material; this guide explains when to read and apply them. If this guide conflicts with an explicit user instruction, follow the user first.

## Work Order

1. Inspect the relevant files, existing conventions, and current Git state.
2. For feature work, bug fixes, refactors, or other multi-step code changes, use Plannable before implementation.
3. Read `MASTER_PLAN.md`, then run `plannable run-next` and load only the returned `plans/PART*_PLAN.ai.md` file.
4. Read every relevant skill's `SKILL.md` before using that skill. Load only the supporting references needed for the active task.
5. Select one primary agent from the table below and give it a self-contained brief.
6. Implement only the active part's outcome. Do not edit unrelated files.
7. Run the listed verification, record evidence, mark the part complete, and run `plannable verify`.
8. Stop after reporting the completed part. Wait for another explicit `run-next` before starting another part.

## Plannable Gate

- The user controls plan execution. Do not start implementation of a Plannable part until the user explicitly says `run-next` or invokes `$plannable run-next`.
- One `run-next` authorizes only the part it returns. Completion of that part does not authorize the next one.
- `MASTER_PLAN.md` and `PLAN_EVIDENCE.md` are the sources of truth. Do not hand-edit `PLAN_STATE.md`; regenerate it with `plannable repair` if it drifts.
- Use `plannable evidence PART-XXX "summary" --artifact "..."` with concrete changed files and check results before `plannable complete PART-XXX`.
- Run `plannable verify` before reporting completion. A passing structural check is not proof of application security.
- The nested `Plannable/` directory has its own Git history and plan. Apply its plan only when working inside that project.

Useful commands:

```powershell
plannable status
plannable run-next
plannable evidence PART-001 "Implemented the active part" --artifact "npm test"
plannable complete PART-001
plannable verify
plannable repair
```

## Skill Loading Contract

When a skill applies, open its `SKILL.md` from the path shown before acting. Do not claim a skill was used unless its instructions were read. If several skills overlap, use the narrowest implementation skill and add design or review skills only when their expertise is needed.

| Skill | Read before use | Use when | Required follow-up |
|---|---|---|---|
| `plannable` | The `plannable` skill supplied by the agent environment, or its `SKILL.md` in the session skill catalog | Planning and executing multi-step work | Read only the active plan part; record evidence before completion |
| `ui-ux-pro-max` | `.claude/skills/ui-ux-pro-max/SKILL.md` | Choosing a UI style, palette, typography, or auditing UX and accessibility | Run `python .claude/skills/ui-ux-pro-max/scripts/search.py "<brief>" --design-system -p "<project>"` before designing, then search the relevant domain or stack |
| `frontend-design` | `.claude/skills/frontend-design/SKILL.md` | Creating a distinctive new interface or reshaping an existing visual direction | Produce a short token and layout plan, critique it against generic defaults, then implement |
| `senior-frontend` | `.claude/skills/senior-frontend/SKILL.md` | React, Next.js, TypeScript, Tailwind, component architecture, or frontend performance work | Read the relevant file under `references/`; inspect a helper script before running it |
| `senior-backend` | `.claude/skills/senior-backend/SKILL.md` | API design, backend business logic, authentication, database access, or backend performance work | Read the relevant file under `references/`; inspect a helper script before running it |
| `mobile-design` | `.claude/skills/mobile-design/SKILL.md` | iOS, Android, React Native, Flutter, or other mobile interface work | Read the required universal references and the applicable platform file, then complete the skill's pre-code checkpoint |
| `code-reviewer` | `.claude/skills/code-reviewer/SKILL.md` | Reviewing a diff, pull request, or risky change | Read the checklist, standards, and anti-pattern references that apply; report findings with severity, risk, and fix |

For `mobile-design`, the required universal references are `mobile-design-thinking.md`, `touch-psychology.md`, `mobile-performance.md`, `mobile-backend.md`, `mobile-testing.md`, and `mobile-debugging.md`. Read `platform-ios.md` for iOS, `platform-android.md` for Android, or both for cross-platform work. Do not write mobile UI code until the skill's checkpoint is complete.

For `senior-frontend`, `senior-backend`, and `code-reviewer`, the `scripts/` and `references/` paths are relative to that skill's directory. Check the script's help and behavior before running it; some helpers create or modify files.

## Agent Workflows

| Agents | Agent Name | Tasks Do |
|---|---|---|
| Planning and coordination | `context-manager` | Captures decisions, prepares concise briefs, maintains context across a long plan, and reports drift or missing information |
| System architecture | `backend-architect` | Chooses API paradigms, service boundaries, data consistency, observability, and security architecture; produces design artifacts rather than implementation code |
| Data architecture | `database-architect` | Designs schemas, indexes, migrations, storage choices, and scaling plans; produces DDL or migration plans rather than application code |
| Frontend delivery | `frontend-developer` | Implements and tests frontend components, state, responsive behavior, and accessibility using the project's existing stack |
| Design review | `ui-ux-designer` | Reviews visual hierarchy, usability, responsive behavior, and accessibility with research-backed recommendations; reports findings and does not edit code |
| Debugging | `debugger` | Reproduces failures, ranks hypotheses, isolates root causes, applies the smallest fix, and adds a regression test when appropriate |
| Quality review | `code-reviewer` | Reviews changed files for correctness, security, performance, maintainability, and test coverage; issues a merge recommendation |

The main coordinating thread owns the plan, dispatches one primary specialist, checks the result, runs verification, and records evidence. If the runtime exposes subagents, pass the brief to the named definition. If it does not, adopt that definition as the working role and state the selection in the report. Never simulate an agent's work or attribute edits to an agent that was not used.

Common routes:

- **UI feature:** `ui-ux-pro-max` and `frontend-design` for direction; `senior-frontend` for implementation guidance; `frontend-developer` to implement; `ui-ux-designer` to review; `code-reviewer` for the final diff.
- **Backend or data change:** `senior-backend` for implementation guidance; `backend-architect` and/or `database-architect` for design decisions. Neither architect definition is a backend implementation agent, so stop and ask the user to assign or define an implementation agent if the active part requires backend code.
- **Bug:** `debugger` to reproduce and isolate; the relevant implementation skill for the fix; `code-reviewer` to check the regression risk.
- **Mobile feature:** `mobile-design` first, then `ui-ux-pro-max` when design direction is open; use `frontend-developer` for React Native work or the appropriate platform specialist if one is later added.
- **Long or multi-part effort:** `context-manager` prepares the active-part brief and keeps the handoff concise; it does not replace the specialist who performs the work.

`frontend-developer` normally asks `context-manager` for project context. When subagent messaging is unavailable, the coordinating thread must supply that context in the brief instead of pretending a request was sent.

## Agent Report Format

Whenever an agent is selected or newly created, report it immediately with this exact column order and then continue the assigned work. Do not pause for acknowledgement solely because an agent was created or assigned.

| Agents | Agent Name | Tasks Do |
|---|---|---|
| One Piece Character | `example-agent` | One concrete assignment |

Use one row per agent. Put the workflow category in `Agents`, the definition name in `Agent Name`, and the active-part assignment in `Tasks Do`. Do not invent a model, effort level, or completed task that the runtime did not expose or perform.

Do not automatically delete, archive, terminate, or otherwise remove created agents, subagents, their threads, worktrees, branches, or review artifacts. Keep them available so the user can review the code and plan. Cleanup or archiving requires an explicit user request, and completion of a task is not authorization to remove them.

## Engineering Guardrails

- Preserve existing behavior and conventions unless the active part changes them deliberately.
- Keep secrets and personal data out of source, plans, logs, and examples; use placeholders.
- Validate inputs at boundaries, use parameterized database access, and keep authorization checks server-side.
- Test changed behavior and failure cases with the project's actual commands. Record unavailable checks honestly.
- Do not add dependencies without authorization, and do not modify unrelated files.
- Prefer accessible, responsive implementations with visible keyboard focus and reduced-motion support.
