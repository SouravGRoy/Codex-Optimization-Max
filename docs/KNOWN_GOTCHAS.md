# Known Gotchas and Protected Behavior

Purpose: record repository-specific failure modes that future agents must know before touching risky areas.

Only add a gotcha when it is verified by code, tests, production behavior, a confirmed bug, or team guidance.
Do not turn guesses into permanent rules.

## How to Add an Entry

Use this format:

### G-XXX — Short title

**Area:** `path/or/system`

**Risk:** What can break?

**Invariant:** What behavior must remain true?

**Evidence:** File/test/PR/issue/log that establishes this.

**Safe approach:** What should an agent inspect or preserve?

**Verification:** How to prove the behavior still works?

---

## High-Risk Areas

These areas require investigation before speculative edits:

- authentication and permissions;
- balances, positions, orders, trades, funding, or financial calculations;
- realtime market data;
- WebSocket connections/subscriptions/reconnect logic;
- shared/global state and caches;
- deployment and CI/CD;
- environment variables and secrets;
- database schema or migrations.

## Realtime / WebSocket Checklist

Before changing realtime code, verify:

- Who creates the connection?
- Is it singleton/shared or per consumer?
- What uniquely identifies a subscription?
- Can the same subscription be created twice?
- Where are listeners removed?
- What happens on component unmount?
- What happens on route/account/market changes?
- What happens after disconnect/reconnect?
- Is backoff centralized?
- Can stale closures read old state?
- Can async work complete after cleanup?
- Is message ordering assumed?
- Are errors retried, surfaced, or swallowed?
- Are there duplicate event handlers?

Do not add arbitrary sleeps, retries, extra connections, or catch-all error swallowing without root-cause evidence.

## Responsive Table / Dense UI Checklist

Before changing tables or dense trading UI, verify:

- minimum and maximum widths;
- content wrapping;
- column sizing strategy;
- horizontal scrolling;
- scrollbar visibility;
- sticky headers/columns;
- tab/navigation width;
- truncation and tooltips;
- long numeric values;
- loading/empty/error states;
- mobile/small desktop widths;
- semantic color indicators.

A visual fix must not change the underlying data semantics.

## Deployment Checklist

Before any merge or production-affecting operation:

- Verify the current branch.
- Verify the target branch.
- Verify whether target-branch updates auto-deploy.
- Verify preview vs production environment.
- Verify the intended Git identity.
- Verify CI/status checks.
- Confirm the exact action with the user.

Never assume a PR preview is equivalent to production.

## Git Identity

Repository-specific Git author/email requirements should be verified with the team and local configuration.
Do not rewrite existing commit authors or force-push merely to change identity without explicit approval.

## Confirmed Gotchas

Add confirmed project-specific entries below.

<!-- Example only — replace/delete after adding real entries.

### G-001 — Shared websocket must not be duplicated

**Area:** `src/...`

**Risk:** Duplicate subscriptions and repeated updates.

**Invariant:** A single shared connection owns subscriptions.

**Evidence:** `src/...`, test `...`, PR `...`.

**Safe approach:** Reuse the existing connection manager; do not instantiate a socket from UI components.

**Verification:** Confirm one connection and one listener per subscription during reconnect.

-->
