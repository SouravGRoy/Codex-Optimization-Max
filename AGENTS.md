# Repository Agent Instructions

## Mission

Work safely inside an existing production codebase.

Priorities, in order:

1. Preserve existing behavior.
2. Follow the repository's established architecture and conventions.
3. Make the smallest change that satisfies the request.
4. Verify the change.
5. Keep Git, PR, deployment, dependency, and infrastructure actions under explicit human control.

Correctness and reviewability are more important than speed.

## Instruction Sources

Before editing, read the relevant repository guidance:

- `docs/ARCHITECTURE.md` for system boundaries and data flow.
- `docs/CONVENTIONS.md` for coding and UI conventions.
- `docs/KNOWN_GOTCHAS.md` for known failure modes and protected behavior.

Treat repository code and tests as stronger evidence than stale documentation.
If guidance conflicts with the actual codebase, stop and report the conflict instead of guessing.

Task-specific skills live in `.agents/skills/`:

- `safe-feature` — feature work and ordinary bug fixes.
- `debugging` — unclear failures, regressions, crashes, async issues, or root-cause analysis.
- `ui-figma` — visual, responsive, CSS, component, or Figma work.
- `safe-git` — branch, commit, push, PR, merge, or history operations.

Use the relevant skill when the task matches it.

## Hard Safety Boundaries

Unless the user explicitly authorizes the exact action in the current task, DO NOT:

- commit;
- push;
- create, merge, close, or modify a pull request;
- merge into `main`, `master`, or another protected branch;
- deploy or trigger a production deployment;
- change Vercel, CI/CD, hosting, DNS, cloud, or infrastructure settings;
- change environment variables, credentials, secrets, auth, permissions, or billing;
- run database migrations or destructive database operations;
- install, remove, or upgrade dependencies;
- rewrite Git history;
- force-push;
- rebase a shared branch;
- delete branches, files, directories, data, or configuration;
- run destructive cleanup commands.

Authorization is narrow:
- approval for one action does not authorize later actions;
- approval in an earlier task does not carry forward;
- "finish it", "ship it", "fix it", or "make it work" does not imply commit, push, merge, or deploy.

If an exact approval is missing, stop before the side effect and ask.

## Scope Discipline

Only change what the request requires.

Do not:
- refactor unrelated code;
- rename unrelated symbols;
- reorganize directories;
- mass-format files;
- "clean up" nearby code;
- optimize unrelated code;
- replace working implementations because another approach seems cleaner;
- introduce abstractions for hypothetical future needs;
- fix unrelated issues discovered during the task.

If an unrelated issue is found, report it separately.

If the task expands beyond the expected scope, stop and explain why before continuing.

## Preflight Before Editing

For any non-trivial task:

1. Inspect `git status`.
2. Identify the current branch.
3. Read the relevant files.
4. Read the applicable docs and skill.
5. Find at least one similar implementation when one exists.
6. Trace important callers/consumers before changing shared code.
7. State a short implementation plan and expected files to change.

Do not treat guesses as facts.

For ambiguous, high-risk, architecture-sensitive, or production-facing work:
perform investigation first and wait for confirmation before editing when a wrong assumption could cause material damage.

## Editing Rules

- Preserve existing naming, formatting, structure, and local patterns.
- Prefer existing helpers, hooks, components, services, types, and utilities.
- Prefer a local patch over a repo-wide change.
- Prefer existing dependencies over adding a new dependency.
- Keep public API changes deliberate and explicit.
- Do not silently change runtime behavior outside the requested path.
- Do not modify generated files unless the project workflow requires it.
- Do not modify these policy files unless the user explicitly asks:
  - `AGENTS.md`
  - `docs/ARCHITECTURE.md`
  - `docs/CONVENTIONS.md`
  - `docs/KNOWN_GOTCHAS.md`
  - `.agents/skills/**`

## Verification

After editing:

1. Inspect `git diff --stat`.
2. Inspect the full relevant diff.
3. Confirm only intended files changed.
4. Run the narrowest relevant existing validation first.
5. Run broader checks only when appropriate and affordable.
6. Distinguish failures caused by the change from pre-existing failures.
7. Check for regressions, debug code, secrets, accidental generated files, and unrelated formatting changes.

Never claim a command, test, build, deployment, or behavior passed unless it was actually verified.

Do not fix unrelated pre-existing lint/test/build failures unless explicitly requested.

## Git Checkpoints

A meaningful, reviewable unit of work is a checkpoint.

At a checkpoint, STOP before starting the next unrelated unit and report:

- **Completed:** what changed.
- **Files changed:** exact files.
- **Verification:** commands/checks actually run and their result.
- **Risks/notes:** anything the reviewer should know.
- **Suggested commit:** a concise commit message.
- **Git status:** whether changes remain uncommitted.

Do not commit or push unless explicitly authorized.

When committing is authorized, stage only intended files when practical; do not default to `git add .` if it could include unrelated work.

## Protected Production Behavior

Treat production-facing code and configuration as high risk.

For changes involving:
- authentication;
- money, balances, orders, trades, positions, or financial calculations;
- real-time market data;
- WebSockets or subscriptions;
- persistence or migrations;
- permissions;
- deployment;
- shared state or caching;

increase verification depth and prefer investigation before implementation.

Do not use a speculative rewrite to solve a production bug.

## Final Self-Review

Before declaring a task complete, ask:

- Did I change anything not required?
- Did I follow an existing pattern?
- Did I preserve behavior outside the task?
- Did I verify the actual failure/success path?
- Did I accidentally change Git, deployment, dependencies, secrets, or infrastructure?
- Is every changed line explainable by the user's request?

If any answer is uncertain, investigate before finishing.
