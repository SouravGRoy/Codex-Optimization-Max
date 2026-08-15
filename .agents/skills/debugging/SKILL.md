---
name: debugging
description: Investigate and fix crashes, regressions, intermittent failures, async bugs, WebSocket/realtime issues, or problems with an unclear root cause. Use when evidence and root-cause analysis are needed before editing; do not start with a broad rewrite.
---

# Debugging

## Core Rule

Evidence before edits.

Do not patch the first plausible symptom.
Do not rewrite working architecture before the failure mechanism is understood.

## Phase 1 — Define the Failure

Capture:
- expected behavior;
- actual behavior;
- reproduction steps;
- affected environment;
- frequency;
- relevant error/log/stack trace;
- last known good behavior when known.

Separate confirmed facts from hypotheses.

## Phase 2 — Inspect the Path

1. Inspect `git status` and current branch.
2. Read the failing code.
3. Trace callers, consumers, state, and cleanup paths.
4. Inspect recent relevant changes when useful.
5. Read tests and known gotchas.
6. Find a similar working path.

Do not edit yet if the root cause is still materially uncertain.

## Phase 3 — Hypotheses

List a small number of plausible causes, ranked by evidence.

For each hypothesis identify:
- evidence supporting it;
- evidence against it;
- cheapest safe way to confirm/refute it.

Prefer observation over speculative code changes.

## Phase 4 — Instrument Carefully

If more evidence is needed:
- use existing logs/devtools/tests first;
- add the smallest temporary instrumentation;
- do not expose secrets or sensitive data;
- do not leave noisy debug logging in the final patch.

Do not change production behavior merely to gather evidence unless explicitly approved.

## Phase 5 — Root Cause

Before implementing a non-trivial fix, be able to explain:

> The failure occurs because **X**, which causes **Y** under **Z** condition.

If that sentence is still guesswork, keep investigating.

## Phase 6 — Fix

- Make the smallest change that fixes the root cause.
- Preserve unrelated behavior.
- Reuse existing patterns.
- Add a focused regression test when the repository has an appropriate test pattern.
- Avoid broad catches, arbitrary delays, blanket retries, state duplication, or silent fallbacks unless they are part of the intended architecture.

## Async / WebSocket / Realtime Checklist

When relevant, inspect:
- connection creation/ownership;
- duplicate connections;
- subscription identity/deduplication;
- listener registration;
- unsubscribe/cleanup;
- component lifecycle;
- reconnect/backoff;
- stale closures;
- race conditions;
- out-of-order messages;
- async completion after unmount;
- changing account/market identifiers;
- shared cache/state updates;
- error and terminal-state handling.

Do not add a second connection or retry loop without proving the current lifecycle requires it.

## Crash Checklist

When relevant:
- locate the first meaningful application frame in the stack;
- inspect nullable/undefined assumptions;
- inspect unhandled promise rejection paths;
- inspect parsing/shape assumptions at external boundaries;
- inspect state transitions leading to the crash;
- distinguish cause from downstream failure.

Do not simply swallow the exception.

## Verification

Prove:
1. the original failure path no longer fails;
2. the surrounding expected behavior still works;
3. the fix did not introduce unrelated changes.

Run the narrowest relevant check first.

If reproduction is unavailable, state the limitation and what evidence supports the fix.

## Final Debug Report

Report:
- Root cause
- Evidence
- Fix
- Files changed
- Verification
- Residual risk / unverified assumptions
- Suggested commit message

Stop before commit, push, PR, merge, or deployment unless explicitly authorized.
