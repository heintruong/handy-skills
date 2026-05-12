---
name: orchestrator-agent
description: Reusable main-orchestrator workflow for implementation plans that must be broken into ordered tasks, delegated to coding agents, reviewed, validated, retried, and documented. Use when the user asks Codex to act as an orchestrator, spawn coding agents, manage task lifecycle, enforce sequential prerequisites, coordinate subagents, or implement a multi-step plan with validation and handoff notes.
---

# Orchestrator Agent

Use this skill to run implementation work as a task-by-task orchestrator. Keep changes minimal, preserve stable contracts, and approve each task only after validation passes.

## Core Responsibilities

1. Analyze the implementation plan and identify ordered executable tasks.
2. Maintain task state: `pending`, `in_progress`, `blocked`, `completed`, `failed`.
3. Work strictly task-by-task; do not skip prerequisites or run unrelated tasks simultaneously.
4. For each task, define the objective, relevant files, constraints, acceptance criteria, and dependencies.
5. Select an execution model:
   - Default: `GPT-5.4-mini`, low reasoning.
   - Use `GPT-5.4`, medium reasoning only for architecture changes, complex debugging, cross-module refactors, concurrency/state management, or security-sensitive logic.
6. Spawn a coding agent for implementation when delegation is useful and explicitly allowed by the user/session.
7. Review the agent output before approval.
8. If validation fails, create a focused revision task that preserves working code.
9. Stop only when all tasks are complete, validations pass, and no blocking issues remain.
10. After completion, create or update concise notes in `.agent/`.

## Task Definition Template

For every task, establish:

- Objective: the specific behavior or artifact to produce.
- Relevant files: only files needed for this task.
- Constraints: stable contracts, scope boundaries, and what not to touch.
- Acceptance criteria: concrete checks that must pass.
- Dependencies: prior tasks or context required before starting.

## Execution Rules

- Inspect existing implementations before modifying patterns.
- Prefer minimal, targeted changes.
- Do not refactor unrelated code.
- Do not introduce unnecessary abstractions.
- Do not assume undocumented behavior.
- Do not modify stable public APIs, shared interfaces/types, database schemas, event payloads, or exported function signatures unless the plan explicitly requires it.
- Run the smallest relevant validation first; prefer targeted checks before full suites.
- Add or update tests when introducing behavior.
- Never remove failing tests unless explicitly required.

## Agent Prompt Requirements

Each coding agent must receive:

- Task objective.
- Relevant files.
- Implementation constraints.
- Expected output.
- Acceptance criteria.
- Reminder that other agents or the user may have changed the codebase; do not revert unrelated edits.

Each coding agent must return:

- Modified files.
- Implementation summary.
- Risks or issues.
- Validation or test results.

## Review Checklist

Before approving a task, validate:

- Acceptance criteria are fully met.
- Implementation is correct and consistent with existing architecture.
- File structure remains coherent.
- Stable contracts are preserved.
- Code style follows local patterns.
- Tests or targeted validation ran when applicable.
- No unrelated behavior or metadata churn was introduced.

## Retry Policy

If validation fails:

1. Identify the exact issue.
2. Create a focused revision task.
3. Preserve correct existing changes.
4. Avoid rewriting working code unnecessarily.
5. Re-review and re-validate before approval.

## Context Management

- Load only files relevant to the current task.
- Avoid replaying completed work; summarize completed tasks.
- Preserve important architectural decisions and constraints between tasks.
- Keep user progress updates concise and factual.

## Post-Process Notes

At completion, create or update `.agent/` notes with:

- Completed work.
- Architecture decisions.
- Important constraints.
- Unresolved issues.
- Useful implementation context for future agents.
- Validation commands and results.
