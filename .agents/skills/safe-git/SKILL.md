---
name: safe-git
description: Safely inspect or perform Git, branch, commit, push, pull-request, merge, or history operations with explicit approval boundaries. Use whenever the task mentions Git, GitHub, branches, commits, PRs, pushing, merging, reverting, rebasing, or deployment through a Git branch.
---

# Safe Git

## Default Mode

Git is read-only unless the user explicitly authorizes a state-changing action.

Safe inspection commands include:
- `git status`
- `git diff`
- `git diff --stat`
- `git log`
- `git show`
- `git branch --show-current`
- `git remote -v`
- `git rev-parse`
- `git merge-base`

Inspection does not authorize mutation.

## Operations Requiring Explicit Approval

Ask before:
- creating/deleting/switching branches when it could disturb work;
- staging files;
- committing;
- amending;
- pushing;
- creating/updating/closing a PR;
- merging;
- rebasing;
- cherry-picking;
- resetting;
- restoring/discarding changes;
- stashing;
- cleaning untracked files;
- rewriting authors/history;
- force-pushing;
- deploying through a branch/merge.

The approval must apply to the exact operation in the current task.

## Never Infer Authorization

These phrases do not automatically authorize Git side effects:
- "finish it"
- "ship it"
- "fix it"
- "make it work"
- "ready it"
- "complete the task"

A request to commit does not automatically authorize push.
A request to push does not automatically authorize merge.
A request to merge does not automatically authorize production deployment unless the deployment consequence is understood and explicitly intended.

## Pre-Mutation Checklist

Before any approved Git mutation:

1. Show current branch.
2. Check `git status`.
3. Identify uncommitted/untracked files.
4. Confirm which files/commits belong to the current task.
5. Confirm remote/target where relevant.
6. Explain any production/deployment consequence.
7. Stop if unrelated work could be affected.

## Branching

When creating a branch is explicitly authorized:
- start from the intended base;
- verify the base is up to date only if pulling/fetching is authorized;
- use a descriptive branch name consistent with repository conventions;
- do not switch branches with unresolved work unless the user approves a safe handling plan.

Do not auto-stash.

## Staging and Commit

When commit is authorized:
- inspect the diff first;
- stage only intended paths/hunks when practical;
- avoid `git add .` if unrelated files exist;
- show the proposed commit message;
- do not include secrets or accidental generated files;
- verify the resulting commit author when identity matters.

After commit, report the commit hash and what remains uncommitted.

## Push

When push is authorized:
- confirm branch and remote;
- push the intended branch only;
- do not force push by default;
- never use `--force` or `--force-with-lease` without explicit approval after explaining why history rewrite is necessary.

## PR

When PR creation is authorized:
- verify base and head branches;
- summarize only verified changes;
- include testing actually performed;
- do not claim checks passed if they were not run;
- do not merge automatically after creating the PR.

## Merge / Production

Treat merge to a production branch as a separate high-risk action.

Before merge:
- verify target branch;
- verify CI/review state;
- verify whether merge auto-deploys;
- summarize production impact;
- obtain explicit approval for the merge.

Do not assume a successful preview deployment makes production safe.

## History Rewrite

For amend, rebase, reset, author rewrite, or force push:

Before doing anything, report:
- commits affected;
- why rewrite is needed;
- whether hashes will change;
- whether remote/shared history is affected;
- whether force push will be required;
- rollback/recovery plan.

Wait for explicit approval.

## Completion Format

Report:
- Current branch
- Working tree status
- Operation performed
- Commit/PR/remote affected
- Verification
- Remaining uncommitted work
- Next state-changing action, if any, requiring approval
