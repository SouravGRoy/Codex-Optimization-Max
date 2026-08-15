# Architecture Guide

> **Status:** Repository-specific template.
>
> Replace bracketed placeholders with verified facts from this repository.
> Codex must not treat placeholder text as architecture.
> Do not invent missing architecture. Inspect the code and record only evidence-backed facts.

## 1. System Overview

**Product purpose:** [FILL: one or two sentences]

**Primary runtime(s):**
- Frontend: [FILL]
- Backend/API: [FILL]
- Realtime/streaming: [FILL]
- Database/storage: [FILL]
- Hosting/deployment: [FILL]

**Primary package manager:** [FILL]

**Production branch:** [FILL AND VERIFY]

## 2. Repository Map

Document only important boundaries, not every folder.

| Area | Path | Responsibility | Notes |
|---|---|---|---|
| App entry | `[FILL]` | [FILL] | [FILL] |
| UI/components | `[FILL]` | [FILL] | [FILL] |
| Domain logic | `[FILL]` | [FILL] | [FILL] |
| API/client layer | `[FILL]` | [FILL] | [FILL] |
| Realtime/WebSocket | `[FILL]` | [FILL] | [FILL] |
| State management | `[FILL]` | [FILL] | [FILL] |
| Shared utilities | `[FILL]` | [FILL] | [FILL] |
| Tests | `[FILL]` | [FILL] | [FILL] |

## 3. Dependency Direction

Record the intended dependency flow.

Example shape only:

```text
UI
  ↓
feature/domain layer
  ↓
services / API / realtime
  ↓
external systems
```

**Verified dependency rules:**
- [FILL]
- [FILL]
- [FILL]

**Forbidden or discouraged dependency directions:**
- [FILL]
- [FILL]

If no rule is verified, inspect similar features before introducing a new dependency direction.

## 4. State Ownership

Document where state should live and who owns it.

| State type | Owner/location | Consumers | Update mechanism |
|---|---|---|---|
| Server data | [FILL] | [FILL] | [FILL] |
| Client UI state | [FILL] | [FILL] | [FILL] |
| Session/auth | [FILL] | [FILL] | [FILL] |
| Realtime market data | [FILL] | [FILL] | [FILL] |
| Cached/derived data | [FILL] | [FILL] | [FILL] |

Rules:
- Do not create a second source of truth without a demonstrated need.
- Do not move state ownership during an unrelated task.
- Trace existing producers and consumers before modifying shared state.

## 5. API and Service Boundaries

**API access pattern:** [FILL]

**Primary client/service modules:** [FILL]

**Error handling pattern:** [FILL]

**Request/response typing:** [FILL]

Rules:
- Reuse the existing client/service layer.
- Do not call external APIs directly from UI code if the repository has a service abstraction.
- Do not change response shapes or shared types without tracing consumers.

## 6. Realtime / WebSocket Architecture

**Connection owner:** [FILL]

**Subscription owner:** [FILL]

**Reconnect strategy:** [FILL]

**Cleanup/unsubscribe path:** [FILL]

**Data fan-out/state path:** [FILL]

Before changing realtime code, verify:
- where connections are created;
- whether connections are shared or per-consumer;
- subscription identity and deduplication;
- cleanup on unmount/logout/market change;
- reconnect and backoff behavior;
- stale closures/state;
- duplicate listeners;
- race conditions;
- ordering assumptions.

Do not add retries, extra connections, or global state as a speculative fix.

## 7. UI / Design System

**Design source:** [FILL: Figma/design system/etc.]

**Shared components:** [FILL]

**Tokens/theme:** [FILL]

**Responsive strategy:** [FILL]

**Table/list primitives:** [FILL]

Rules:
- Reuse existing design-system primitives.
- Preserve functionality while making visual fixes.
- Avoid one-off CSS when an established token/component solves the same problem.

## 8. Critical Data Flows

Document only flows where architecture mistakes would be costly.

### [Flow name]

```text
[FILL source]
  → [FILL transform/service]
  → [FILL state/cache]
  → [FILL consumer]
```

**Invariants:**
- [FILL]
- [FILL]

Repeat this section for:
- authentication/session;
- realtime data;
- order/trade/position flows;
- other critical product paths.

## 9. Build, Test, and Validation

Discover commands from repository files such as `package.json`, Makefile, task runner config, and CI.

- Install: `[FILL]`
- Dev: `[FILL]`
- Lint: `[FILL]`
- Typecheck: `[FILL]`
- Unit tests: `[FILL]`
- Integration tests: `[FILL]`
- Build: `[FILL]`

Do not invent commands.

## 10. Deployment

**Deployment provider:** [FILL]

**Production branch:** [FILL AND VERIFY]

**Preview deployment behavior:** [FILL]

**Environment separation:** [FILL]

Safety:
- A merge to a production branch may trigger production automatically.
- Never merge or deploy merely because implementation is complete.
- Verify the target branch and deployment consequence before any production-affecting action.

## 11. Reference Implementations

For each common task, record one good existing example.

| Task | Reference |
|---|---|
| New page/view | `[FILL path]` |
| Data-fetching component | `[FILL path]` |
| Mutation/action | `[FILL path]` |
| Realtime subscription | `[FILL path]` |
| Responsive table | `[FILL path]` |
| Modal/form | `[FILL path]` |
| Error state | `[FILL path]` |

When implementing something new, prefer these references over generic framework boilerplate.

## 12. Architecture Decisions / Constraints

Keep short, verified entries.

### ADR-lite: [Title]
- **Decision:** [FILL]
- **Reason:** [FILL]
- **Do not:** [FILL]
- **Reference:** [FILL commit/PR/file if available]

Add entries only when they materially reduce future architectural guessing.
