---
name: safe-feature
description: Implement a focused feature or ordinary bug fix in an existing production repository with minimal changes, architecture preservation, verification, and no automatic Git or deployment side effects. Use for normal implementation work; use debugging instead when the root cause is unclear.
---

# Safe Feature

## Goal

Deliver the requested behavior with the smallest reviewable patch while preserving architecture and unrelated behavior.

## Before Editing

1. Read the applicable `AGENTS.md`.
2. Read relevant sections of:
   - `docs/ARCHITECTURE.md`
   - `docs/CONVENTIONS.md`
   - `docs/KNOWN_GOTCHAS.md`
3. Inspect `git status` and current branch.
4. Read the target code and important callers/consumers.
5. Find a similar existing implementation when available.
6. Identify the smallest set of files expected to change.
7. State a short plan.

If the request is ambiguous in a way that could produce materially different behavior, ask before editing.

## Implementation Rules

- Follow existing patterns rather than generic framework boilerplate.
- Reuse existing components, hooks, services, utilities, and types.
- Prefer a local patch over architectural change.
- Do not refactor, rename, reorganize, optimize, or clean up unrelated code.
- Do not add dependencies without explicit approval.
- Do not change public/shared interfaces unless required.
- Do not broaden scope when you discover nearby issues.
- Preserve loading, error, empty, accessibility, and responsive behavior unless the task changes them.

If implementation becomes significantly larger than planned, stop and explain why.

## High-Risk Escalation

Switch to investigation-first behavior when the task touches:
- auth/permissions;
- money or financial calculations;
- orders/trades/positions/balances;
- WebSockets/realtime data;
- shared state/cache;
- migrations;
- deployment/infrastructure.

Do not use a speculative rewrite in these areas.

## Verification

After editing:

1. Inspect `git diff --stat`.
2. Inspect the relevant full diff.
3. Verify there are no unrelated changes.
4. Run the narrowest relevant existing check.
5. Run broader checks when appropriate.
6. Perform a regression-focused self-review.

Never hide failures or fix unrelated pre-existing failures.

## Completion

Stop at the first meaningful checkpoint.

Report:
- Completed
- Files changed
- Verification performed and result
- Risks/notes
- Suggested commit message
- Current Git status

Do not commit, push, create/merge a PR, or deploy unless explicitly authorized.
