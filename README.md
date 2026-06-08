# Architect Builder

**English** | [简体中文](README.zh-CN.md)

> Stop letting one AI plan, code, and judge itself. Architect Builder separates AI-assisted development into explicit Architect -> Builder -> Reviewer handoff files.

[![Claude Code Plugin](https://img.shields.io/badge/Claude%20Code-plugin-5B4BFF)](https://code.claude.com/docs/en/plugins)
[![Codex Skills](https://img.shields.io/badge/Codex-skills-111111)](https://openai.com/codex/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## Quick Install

```text
/plugin marketplace add bubble0462/Architect-Builder
/plugin install architect-builder@architect-builder
```

Then use:

```text
/architect-builder:plantask
/architect-builder:executetask
/architect-builder:reviewtask
```

Architect Builder is a lightweight Codex skill workflow for separating AI-assisted software work into three explicit phases:

1. **Plan** with `plantask`
2. **Build** with `executetask`
3. **Review** with `reviewtask`

The goal is to make AI coding work more controlled, auditable, and repeatable. Instead of asking one model to freely plan, implement, and judge its own work, this workflow creates handoff files under `.ai/` and uses them as a contract between planning, execution, and review.

## Why Use This?

AI coding agents are useful, but long-running sessions often drift:

- The model expands the scope.
- The implementation no longer matches the original request.
- The same model reviews its own assumptions.
- Tests and acceptance criteria are added after the fact, or skipped entirely.

Architect Builder makes each phase explicit:

- `plantask` defines the scope before implementation.
- `executetask` builds only what the task files specify.
- `reviewtask` checks the result against the original plan, diff, and acceptance criteria.

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
  .claude-plugin/
    marketplace.json
    plugin.json
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

### Claude Code Plugin Marketplace

Claude Code can install this repository as a plugin marketplace:

```text
/plugin marketplace add bubble0462/Architect-Builder
```

Then install the plugin:

```text
/plugin install architect-builder@architect-builder
```

After installation, the skills are available under the plugin namespace:

```text
/architect-builder:plantask
/architect-builder:executetask
/architect-builder:reviewtask
```

### Codex Local Skills

Clone this repository first:

```powershell
cd D:\Users\bubble\Desktop
git clone https://github.com/bubble0462/Architect-Builder.git
cd Architect-Builder
```

Then copy the skill folders into your local Codex skills directory:

```powershell
Copy-Item -Recurse .\skills\plantask "$env:USERPROFILE\.codex\skills\plantask"
Copy-Item -Recurse .\skills\executetask "$env:USERPROFILE\.codex\skills\executetask"
Copy-Item -Recurse .\skills\reviewtask "$env:USERPROFILE\.codex\skills\reviewtask"
```

If a skill already exists, back it up first or add `-Force` only when you intentionally want to overwrite it.

Why copy instead of installing directly from GitHub? Codex loads local skills from your `.codex/skills` directory. This repository is the source package; copying puts the skills where Codex can discover them while keeping the repository separate from your active local skill directory.

### Local Claude Code Plugin Test

If you want to test the plugin locally before installing from GitHub:

```powershell
claude --plugin-dir .\Architect-Builder
```

The marketplace metadata lives in `.claude-plugin/marketplace.json`, and the plugin manifest lives in `.claude-plugin/plugin.json`.

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

- These skills can be used as local Codex skills or as a Claude Code marketplace plugin.
- They do not require runtime dependencies.
- They are designed to be copied into your local `.codex/skills` directory.
- Keep project-specific `.ai/` files inside each project, not inside this repository.

## Suggested GitHub Topics

If you fork or publish a related workflow, these topics make it easier to discover:

```text
claude-code, claude-plugin, codex, ai-agent, agentic-coding, skills, code-review, ai-workflow, software-engineering
```
