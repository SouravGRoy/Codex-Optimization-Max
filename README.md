# Codex Repository Safety & Workflow

This repository is configured to make Codex work safely inside an existing production codebase.

The goal of this setup is to improve:

* architecture consistency
* code quality
* debugging reliability
* scope control
* UI consistency
* Git safety
* production safety
* repeatability across Codex sessions

Codex should use the repository documentation and skills before making changes instead of relying on generic assumptions.

---

## Repository Structure

```text
repo/
├── AGENTS.md
├── docs/
│   ├── ARCHITECTURE.md
│   ├── CONVENTIONS.md
│   └── KNOWN_GOTCHAS.md
│
└── .agents/
    └── skills/
        ├── safe-feature/
        │   └── SKILL.md
        ├── safe-git/
        │   └── SKILL.md
        ├── ui-figma/
        │   └── SKILL.md
        └── debugging/
            └── SKILL.md
```

---

# How the System Works

The files are split into four responsibilities:

```text
AGENTS.md
    ↓
Global safety and behavior rules

docs/
    ↓
Project knowledge and architecture

.agents/skills/
    ↓
Task-specific workflows

User prompt
    ↓
The exact task to perform
```

The intended priority is:

```text
Safety rules
    ↓
Project architecture
    ↓
Project conventions
    ↓
Known risks/gotchas
    ↓
Relevant skill
    ↓
Current task
```

---

# `AGENTS.md`

`AGENTS.md` contains the rules Codex must follow for all work in this repository.

It defines things such as:

* follow existing architecture
* preserve existing behavior
* make minimal changes
* do not refactor unrelated code
* do not perform unnecessary cleanup
* investigate before making risky changes
* verify work before considering it complete
* stop at meaningful development checkpoints
* never perform state-changing Git operations

This file should contain rules that apply to nearly every task.

Do not use it as a dumping ground for detailed project architecture or task-specific procedures.

---

# `docs/ARCHITECTURE.md`

This file describes how the actual application is structured.

It should document verified facts such as:

* application entry points
* major folders and responsibilities
* service boundaries
* state ownership
* API/data-access patterns
* realtime/WebSocket architecture
* authentication flow
* important data flows
* shared UI architecture
* deployment assumptions
* reference implementations

The purpose of this file is to prevent Codex from inventing architecture.

When implementing something new, Codex should first determine how similar functionality is already implemented.

Example:

```text
UI Component
    ↓
UI-facing hook
    ↓
Context/provider
    ↓
Platform service
    ↓
External SDK/API
```

If the project already follows this flow, Codex should not bypass it by calling an external API directly from a UI component.

Only verified architecture should be documented.

If something cannot be verified, it should be marked as unknown rather than guessed.

---

# `docs/CONVENTIONS.md`

This file describes how code should look and behave inside the repository.

It should contain verified conventions such as:

* file naming
* component naming
* hook patterns
* TypeScript conventions
* import patterns
* state-management patterns
* API/service usage
* error handling
* WebSocket conventions
* responsive UI conventions
* formatting/lint rules
* dependency rules
* testing expectations

The purpose is not to describe generic React or TypeScript best practices.

It should describe how **this repository already works**.

Existing code and configuration remain the source of truth.

---

# `docs/KNOWN_GOTCHAS.md`

This file is the long-term memory for things that are easy to break.

Examples:

```text
- A WebSocket must not be created directly inside UI components.
- Changing markets must unsubscribe the previous subscription.
- Position Size color represents Long/Short direction.
- The asset table intentionally uses horizontal scrolling.
- A merge to main automatically triggers production deployment.
```

Use this file when:

* a bug has happened before
* there is an unusual architectural constraint
* a fix introduced an important invariant
* an implementation detail is easy for future agents to misunderstand

Do not fill it with generic programming advice.

Each gotcha should ideally include:

* affected area
* risk
* invariant
* evidence
* safe approach
* verification method

---

# Skills

Skills define repeatable workflows for specific types of tasks.

Instead of putting every possible instruction into `AGENTS.md`, Codex should use the relevant skill when needed.

---

## `safe-feature/SKILL.md`

Use for:

* normal feature work
* small functional additions
* ordinary bug fixes where the cause is reasonably understood
* API/UI integration work that does not require deep debugging

Typical workflow:

```text
Inspect
    ↓
Find existing pattern
    ↓
Define scope
    ↓
Implement minimal solution
    ↓
Verify
    ↓
Review diff
    ↓
Stop at checkpoint
```

The skill should prevent:

* unnecessary abstractions
* repo-wide rewrites
* unrelated refactoring
* scope expansion
* accidental architecture changes

---

## `debugging/SKILL.md`

Use when the root cause is not yet known.

Examples:

* crashes
* WebSocket leaks
* stale data
* duplicate subscriptions
* race conditions
* inconsistent state
* intermittent failures
* unexpected API behavior
* production regressions

The core rule is:

```text
Evidence before edits.
```

The expected workflow is:

```text
Reproduce
    ↓
Trace execution/data flow
    ↓
Build hypotheses
    ↓
Gather evidence
    ↓
Confirm root cause
    ↓
Propose smallest fix
    ↓
Implement
    ↓
Verify original failure
```

Codex should not immediately patch the first plausible symptom.

For risky bugs, investigation and implementation should normally be separated into two steps.

---

## `ui-figma/SKILL.md`

Use for:

* Figma implementation
* responsive issues
* spacing/alignment
* overflow
* clipping
* table layout
* typography
* tabs/navigation
* visual consistency

The main rule is:

```text
Change presentation without changing behavior.
```

Codex should preserve:

* business logic
* data semantics
* state ownership
* interactions
* accessibility
* existing responsive architecture

It should not redesign unrelated parts of the page while fixing one UI issue.

---

## `safe-git/SKILL.md`

Git is intentionally **manual-only**.

Codex may inspect Git using read-only commands such as:

```bash
git status
git diff
git diff --stat
git log
git show
git branch --show-current
git remote -v
```

Codex must never execute state-changing Git operations.

That includes:

```bash
git add
git commit
git push
git pull
git merge
git rebase
git reset
git restore
git checkout
git switch
git stash
git clean
git cherry-pick
```

It must also never directly:

* create a PR
* merge a PR
* delete a branch
* rewrite Git history
* force push
* deploy production

When Git work is needed, Codex provides the exact commands and the user runs them manually.

The separation is intentional:

```text
Codex
    ↓
writes + verifies code
    ↓
provides Git commands
    ↓
STOP

User
    ↓
reviews diff
    ↓
runs Git commands manually
```

---

# Recommended Daily Workflow

For every meaningful task:

```text
1. Create/select the task branch manually
        ↓
2. Start a fresh Codex conversation
        ↓
3. Describe one focused task
        ↓
4. Codex reads repository instructions
        ↓
5. Investigate existing implementation
        ↓
6. Propose approach
        ↓
7. Implement
        ↓
8. Verify
        ↓
9. Review diff
        ↓
10. Stop at meaningful checkpoint
        ↓
11. Codex gives Git commands
        ↓
12. User manually commits/pushes
        ↓
13. Continue with next unit of work
```

Avoid putting several unrelated tasks into one long Codex conversation.

A fresh conversation per logical task reduces context drift and incorrect assumptions.

---

# Recommended Default Prompt

For most development tasks:

```text
Read and follow AGENTS.md and use the relevant repository skill.

Task:
[DESCRIBE TASK]

Inspect the existing implementation and similar patterns first.

Follow the existing architecture and conventions.
Make the smallest production-safe change.
Do not modify unrelated code.

Stop at the first meaningful checkpoint.

Never execute state-changing Git commands.
Give me the commands to run manually when the checkpoint is ready.
```

---

# Recommended Investigation-First Prompt

Use this for risky or unclear work:

```text
Read AGENTS.md and use the debugging skill.

Investigate this issue first:

[DESCRIBE ISSUE]

Do not edit anything yet.

Trace the existing implementation and determine:

- current behavior
- expected behavior
- likely root cause
- files involved
- smallest safe fix
- regression risks

Do not guess.

Wait for my approval before implementing.
```

This is especially recommended for:

* WebSockets
* authentication
* financial logic
* trading
* balances
* account data
* deposits/withdrawals
* shared state
* production regressions

---

# Meaningful Checkpoints

Codex should stop after a meaningful, reviewable unit of work.

Good checkpoints include:

* one bug fully fixed
* one component completed
* one API integration completed
* one responsive issue resolved
* one table/tab fixed
* one isolated feature completed

Bad checkpoints include:

* changing one CSS value
* renaming one variable
* arbitrary commits created only to increase GitHub activity

At a checkpoint, Codex should report:

```text
Completed:
Files changed:
Verification:
Risks / notes:
Suggested commit message:
Git commands for user:
```

Then stop.

---

# Production Safety

This repository should be treated as production software.

Extra caution is required around:

* authentication
* wallet signing
* balances
* trading
* orders
* positions
* deposits
* withdrawals
* financial calculations
* WebSockets
* market data
* shared state
* environment variables
* CI/CD
* Vercel
* deployment

For these areas:

```text
Investigate
    ↓
Understand
    ↓
Implement smallest fix
    ↓
Verify deeply
```

Never use speculative rewrites in high-risk areas.

---

# Keeping the Documentation Useful

These files should evolve with the project.

Update `ARCHITECTURE.md` when:

* architecture genuinely changes
* a new service boundary is introduced
* state ownership changes
* a major data flow is added

Update `CONVENTIONS.md` when:

* the team adopts a new established pattern
* tooling/configuration changes
* an existing convention becomes obsolete

Update `KNOWN_GOTCHAS.md` when:

* a production bug reveals an important invariant
* Codex repeatedly misunderstands something
* a subsystem has a non-obvious constraint

Update skills only when the workflow itself should change.

Do not automatically rewrite these files during ordinary feature work.

---

# Core Philosophy

The system is designed around four layers:

```text
AGENTS.md
"How must the agent behave?"

ARCHITECTURE.md + CONVENTIONS.md + KNOWN_GOTCHAS.md
"How does this repository actually work?"

SKILL.md
"How should this type of task be performed?"

User prompt
"What exactly needs to be done right now?"
```

The guiding principle is:

> **Preserve first. Investigate before guessing. Make the smallest correct change. Verify it. Keep Git and production under human control.**
