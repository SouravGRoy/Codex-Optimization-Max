# Repository Conventions

> Record repository-specific conventions here after verifying them in code.
> Existing code, tests, formatter/linter configuration, and established patterns are the source of truth.
> Do not invent a convention just because it is common in another project.

## 1. General Change Philosophy

- Prefer the smallest correct patch.
- Preserve existing behavior unless the task explicitly changes it.
- Match the local file/module style before applying a global preference.
- Reuse existing utilities, components, hooks, services, and types.
- Avoid unrelated refactors, cleanup, renames, or formatting.
- Avoid introducing abstractions until the current codebase demonstrates a repeated need.
- Avoid new dependencies when the repository already has a suitable solution.

## 2. Naming

Record verified patterns:

- Files: [FILL]
- React/components: [FILL]
- Hooks: [FILL]
- Functions: [FILL]
- Constants: [FILL]
- Types/interfaces: [FILL]
- API/service methods: [FILL]
- Tests: [FILL]

When unsure, inspect neighboring files and a similar feature.

## 3. TypeScript / JavaScript

Verified settings:
- Strict mode: [FILL]
- Import aliases: [FILL]
- Type-only import convention: [FILL]
- `any` policy: [FILL]
- Error typing pattern: [FILL]

Defaults until verified:
- Preserve existing types rather than weakening them to make errors disappear.
- Do not add `any`, broad casts, `@ts-ignore`, or `@ts-expect-error` as a shortcut.
- Do not change shared types without tracing consumers.
- Keep domain types close to the repository's established location.

## 4. Imports and Module Boundaries

- Follow existing alias/relative-import patterns.
- Do not introduce circular dependencies.
- Do not reach across feature boundaries when a public/shared API already exists.
- Do not move files solely to make imports look cleaner.
- Preserve import ordering according to existing formatter/linter behavior.

## 5. React / UI Components

Verified component style: [FILL]

Rules:
- Preserve component responsibilities.
- Prefer existing shared primitives over creating near-duplicates.
- Do not move data fetching, state, or side effects into presentation components unless that matches an existing pattern.
- Preserve accessibility and keyboard behavior.
- Keep loading, error, empty, and disabled states consistent with nearby features.
- Avoid component rewrites for purely visual fixes.

## 6. State and Side Effects

Verified state libraries/patterns: [FILL]

Rules:
- Keep a single source of truth.
- Do not duplicate server/realtime state into new local/global stores without need.
- Trace effects and cleanup before changing lifecycle-sensitive code.
- Avoid adding effects merely to synchronize state that can be derived.
- Do not change cache invalidation or subscription semantics casually.

## 7. API / Data Access

Verified pattern: [FILL]

Rules:
- Use the established client/service abstraction.
- Preserve error and loading semantics.
- Preserve request cancellation/cleanup patterns.
- Do not silently change API shapes.
- Validate assumptions about nullable/optional fields against existing types and usage.

## 8. Realtime / WebSocket

Verified pattern: [FILL]

Rules:
- Respect existing connection ownership.
- Avoid duplicate connections and duplicate subscriptions.
- Always preserve cleanup/unsubscribe behavior.
- Treat reconnect/backoff changes as behavior changes requiring focused verification.
- Be cautious with stale closures, changing dependency arrays, and shared mutable state.
- Do not "fix" realtime problems by adding arbitrary delays/retries without evidence.

## 9. Styling / Figma

Verified styling system: [FILL]

Rules:
- Reuse tokens, variables, primitives, and established responsive breakpoints.
- Match the existing design system before using hard-coded values.
- Do not redesign areas outside the requested target.
- Preserve content semantics and business logic during visual changes.
- Check overflow, clipping, wrapping, scroll behavior, sticky elements, and small widths.
- Avoid global CSS changes for a local issue unless the architecture clearly requires it.

## 10. Error Handling

Verified pattern: [FILL]

Rules:
- Preserve meaningful errors.
- Do not swallow exceptions just to stop a crash.
- Do not replace a root-cause fix with a broad `try/catch`.
- Log only according to existing conventions.
- Remove temporary debug logging before completion unless explicitly requested.

## 11. Tests

Verified test stack: [FILL]

Rules:
- Add or update tests when behavior changes and the repository has a relevant testing pattern.
- Prefer focused regression tests for bugs.
- Avoid brittle tests tied only to implementation details.
- Do not rewrite unrelated tests to make a change pass.
- Report pre-existing failures separately.

## 12. Formatting

Verified formatter/linter: [FILL]

Rules:
- Do not run a whole-repo formatter for a small change.
- Do not create large whitespace-only diffs.
- Preserve line endings and file structure.
- If tooling auto-formats unrelated code, revert unrelated formatting before finishing.

## 13. Comments and Documentation

- Explain non-obvious constraints, not obvious syntax.
- Do not add verbose AI-style comments.
- Preserve useful existing comments.
- Update architecture/convention docs only when the task explicitly includes documentation or a verified architectural rule changed.

## 14. Dependencies

- Do not install, remove, or upgrade dependencies without explicit approval.
- Before proposing a new package, show why existing dependencies or platform APIs are insufficient.
- Never change lockfiles accidentally.

## 15. Security / Secrets

- Never print, paste, commit, or expose secrets.
- Do not modify `.env*` files unless explicitly authorized and necessary.
- Do not weaken auth, permissions, CSP, validation, or security controls to make a feature work.
- Treat security-sensitive changes as high-risk and require deeper review.

## 16. Definition of Done

A code change is not complete until:
- requested behavior is implemented;
- unrelated behavior is preserved;
- the diff is scoped;
- relevant checks were run;
- temporary/debug code is removed;
- no unintended dependency/config/lockfile changes exist;
- Git/PR/deploy side effects remain under explicit user control.
