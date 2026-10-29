# Monorepo Template

This workspace holds [Plannable](Plannable/README.md), a planning CLI and skill for AI coding agents, along with a shared set of Claude Code agents and skills used while working on it.

Plannable turns a product idea into a short, human-readable `MASTER_PLAN.md` and a set of compressed plan parts (`plans/PART*_PLAN.ai.md`, in the PlannablePlan format). An agent loads one part at a time, and a part is marked complete only after evidence has been recorded in `PLAN_EVIDENCE.md`. The CLI is written in TypeScript, targets Node.js 22+, and has no runtime dependencies. `Plannable/site` contains the project's documentation website.

The `.claude` folder at the root holds reusable agent definitions (for example `code-reviewer` and `debugger`) and skills (for example `ui-ux-pro-max` and `senior-backend`) that Claude Code can use anywhere in this workspace.

## Folder structure

```txt
Monorepo-Template/
├── .claude/
│   ├── agents/                 # Claude Code subagent definitions (code-reviewer, debugger, architects, designers)
│   └── skills/                 # Shared skills (code-reviewer, frontend-design, mobile-design, senior-backend,
│                               #   senior-frontend, ui-ux-pro-max) with scripts, references, and data
├── Plannable/                  # Plannable CLI and skill (nested Git repository)
│   ├── src/
│   │   ├── cli.ts              # CLI entry point and command routing
│   │   ├── commands/           # One file per command: create, run-next, status, verify, evidence,
│   │   │                       #   complete, compress, expand, doctor, repair, init
│   │   └── core/               # Plan parsing and rendering, state, evidence, progress, filesystem helpers
│   │       ├── guidance/       # Selection and rendering of safeguard guidance modules
│   │       └── scenario-hints/ # Domain-specific nudges used when generating plans
│   ├── templates/              # Plan file templates and guidance JSON (access, data, input, quality, ...)
│   ├── tests/                  # Vitest suites for the CLI, core logic, guidance, and installation
│   ├── scripts/                # Package check and guidance sync scripts
│   ├── docs/                   # Commands, spec, workflows, install, release, and audit docs
│   ├── examples/               # Sample plans (crm, todo-app, restaurant-homepage)
│   ├── plans/                  # Plannable's own plan parts, with MASTER_PLAN.md and PLAN_*.md at its root
│   ├── site/                   # Documentation website (Next.js-style app built with vinext, Cloudflare worker)
│   ├── .agents/ .codex/        # Plannable skill packaged for Codex
│   ├── .claude/                # Plannable skill packaged for Claude Code
│   ├── .cursor/                # Plannable skill, rules, and slash commands for Cursor
│   └── .github/                # CI and release workflows, issue and PR templates
└── README.md
```

## Getting started

```bash
cd Plannable
npm ci
npm run build
npm run check   # format, lint, typecheck, tests, build, and package check
```

See [Plannable/README.md](Plannable/README.md) for usage and [Plannable/AGENTS.md](Plannable/AGENTS.md) for agent conventions.
