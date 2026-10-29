# AGENTS.md

## Purpose

This document defines the behavior, standards, responsibilities, workflows,
and development rules for all AI agents and contributors working in this project.

The goal is to maintain:
- Consistent architecture
- Clean code standards
- Scalable systems
- Predictable workflows
- Reusable development patterns
- Professional collaboration

---

# Core Principles

- Write clean and maintainable code
- Prioritize readability over cleverness
- Keep architecture scalable
- Follow separation of concerns
- Reuse components and services when possible
- Avoid unnecessary complexity
- Follow project conventions strictly
- Document important decisions
- Maintain security best practices
- Optimize only when necessary

---

# General Development Rules

## Code Quality

- Use meaningful variable and function names
- Avoid hardcoded values
- Keep functions small and focused
- Remove unused code
- Prevent duplicate logic
- Prefer composition over inheritance
- Keep files organized and modular

---

## Project Structure

- Follow feature-based organization when possible
- Separate:
  - Business logic
  - UI
  - Database logic
  - Validation
  - Services
  - Utilities

- Keep shared resources reusable
- Maintain a predictable folder structure

---

# Workflow Standards

## Before Starting Work

Agents must:
1. Understand the task fully
2. Review existing architecture
3. Check related files before editing
4. Avoid breaking existing functionality
5. Ask for clarification if requirements are unclear

---

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

---

## During Development

Agents should:
- Make incremental changes
- Keep commits focused
- Follow existing coding style
- Reuse existing utilities/components
- Maintain backward compatibility when possible

---

## Before Finishing

Always:
- Test functionality
- Check for errors/warnings
- Review formatting
- Ensure imports are clean
- Verify no unrelated files were modified
- Update documentation if needed

---

# Git Standards

## Branch Naming

Use:
- feature/feature-name
- fix/bug-name
- refactor/module-name
- chore/task-name

Examples:
- feature/user-authentication
- fix/login-validation
- refactor/payment-service

---

## Commit Message Convention

Format:

type(scope): short description

Examples:
- feat(auth): add JWT authentication
- fix(api): resolve validation bug
- refactor(db): optimize query handling
- docs(setup): update installation guide

---

# Pull Request Rules

## PR Requirements

Every PR should:
- Focus on one feature or fix
- Include a clear description
- Avoid unrelated changes
- Pass tests before merging

---

## PR Checklist

- [ ] Code tested
- [ ] No console errors
- [ ] No unused imports
- [ ] Documentation updated
- [ ] Formatting checked
- [ ] No sensitive data exposed

---

# Security Standards

Never:
- Expose API keys
- Commit `.env` files
- Store secrets in frontend code
- Trust client-side validation alone

Always:
- Validate input properly
- Sanitize user data
- Use environment variables
- Apply authentication & authorization checks

---

# Performance Standards

- Optimize database queries
- Avoid unnecessary API calls
- Lazy load when appropriate
- Prevent memory leaks
- Minimize redundant renders
- Cache expensive operations if needed

---

# Documentation Rules

Document:
- Complex business logic
- API behavior
- Environment setup
- Deployment steps
- Important architectural decisions

Avoid documenting obvious code.

---

# Testing Standards

## Minimum Requirements

Agents should test:
- Core functionality
- Validation logic
- Edge cases
- API responses
- Error handling

---

## Testing Philosophy

- Test behavior, not implementation
- Keep tests readable
- Avoid flaky tests
- Prefer maintainable test structures

---

# Architecture Guidelines

## Preferred Practices

- Use modular architecture
- Keep services independent
- Use DTOs/schemas for validation
- Keep controllers thin
- Move business logic into services

---

## Scalability Rules

Design systems that:
- Can grow without major rewrites
- Support future features
- Remain maintainable over time

---

# Database Standards

- Use proper indexing
- Normalize appropriately
- Use migrations consistently
- Avoid destructive queries without backups
- Keep database naming conventions consistent

---

# API Standards

## REST Guidelines

Use:
- Proper HTTP methods
- Consistent response formats
- Proper status codes
- Validation for all requests

---

## API Response Format

Example:

```json
{
  "success": true,
  "message": "Request successful",
  "data": {}
}

```
