# Architect Builder

**English** | [简体中文](README.zh-CN.md)

Architect Builder is a lightweight Codex skill workflow for separating AI-assisted software work into three explicit phases:

1. **Plan** with `plantask`
2. **Build** with `executetask`
3. **Review** with `reviewtask`

The goal is to make AI coding work more controlled, auditable, and repeatable. Instead of asking one model to freely plan, implement, and judge its own work, this workflow creates handoff files under `.ai/` and uses them as a contract between planning, execution, and review.

## Skills

| Skill | Purpose | Output |
|---|---|---|
| `plantask` | Creates a technical plan and task handoff files | `AGENTS.md`, `.ai/PLAN.md`, `.ai/TASK.md`, `.ai/ACCEPTANCE.md` |
| `executetask` | Executes the planned tasks and records implementation details | `.ai/IMPLEMENTATION.md`, `.ai/IMPLEMENTATION.diff`, git commit |
| `reviewtask` | Reviews implementation against the plan and acceptance criteria | `.ai/REVIEW.md` |

## Recommended Workflow

```text
Codex + plantask
  -> creates the plan, task list, and acceptance criteria

Builder model + executetask
  -> implements only what the task files specify

Codex + reviewtask
  -> reviews the diff, checks scope, tests, risks, and merge readiness
```

This works especially well when you want one model to act as the architect/reviewer and another model to act as the builder.

## Repository Layout

```text
Architect-Builder/
  README.md
  README.zh-CN.md
  skills/
    plantask/
      SKILL.md
    executetask/
      SKILL.md
    reviewtask/
      SKILL.md
```

## Installation

Copy the skill folders into your Codex skills directory:

```powershell
Copy-Item -Recurse .\skills\plantask "$env:USERPROFILE\.codex\skills\plantask"
Copy-Item -Recurse .\skills\executetask "$env:USERPROFILE\.codex\skills\executetask"
Copy-Item -Recurse .\skills\reviewtask "$env:USERPROFILE\.codex\skills\reviewtask"
```

If a skill already exists, back it up first or add `-Force` only when you intentionally want to overwrite it.

## Usage

### 1. Plan

Use this when you want Codex to inspect a project and create a controlled implementation plan.

```text
plantask
```

Expected files:

```text
AGENTS.md
.ai/PLAN.md
.ai/TASK.md
.ai/ACCEPTANCE.md
```

### 2. Execute

Use this in the same project after the plan is created.

```text
executetask
```

The executor must read `.ai/PLAN.md`, `.ai/TASK.md`, and `.ai/ACCEPTANCE.md`, then implement only the planned tasks.

Expected files:

```text
.ai/IMPLEMENTATION.md
.ai/IMPLEMENTATION.diff
```

### 3. Review

Use this after implementation.

```text
reviewtask
```

The reviewer checks the implementation against the original plan, task list, acceptance criteria, implementation notes, and git diff.

Expected file:

```text
.ai/REVIEW.md
```

## Why This Exists

AI coding can drift when planning, implementation, and review happen in one uninterrupted prompt. Architect Builder reduces that drift by making each phase explicit:

- The plan defines scope.
- The task file defines execution boundaries.
- The acceptance file defines how success is judged.
- The implementation file records what actually changed.
- The review file decides whether the result is acceptable.

## Notes

- These skills are plain local Codex skills.
- They do not require runtime dependencies.
- They are designed to be copied into your local `.codex/skills` directory.
- Keep project-specific `.ai/` files inside each project, not inside this repository.

