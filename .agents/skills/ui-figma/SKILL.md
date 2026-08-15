---
name: ui-figma
description: Make focused UI, CSS, responsive-layout, table, component, or Figma-matching changes while preserving existing behavior and design-system conventions. Use for visual mismatches, overlap, clipping, spacing, tabs, tables, responsiveness, and pixel-accuracy work; do not use for unrelated data/business-logic changes.
---

# UI / Figma

## Principle

Change presentation without accidentally changing product behavior.

Figma/design references define the intended visual target.
Existing code defines current data, interaction, and business behavior unless the request explicitly changes it.

## Before Editing

1. Read applicable repository instructions and UI conventions.
2. Inspect the target component and its parent/container.
3. Inspect relevant shared UI primitives, tokens, and styles.
4. Find a similar correctly implemented component.
5. Identify whether the bug originates in:
   - container sizing;
   - flex/grid behavior;
   - intrinsic content width;
   - overflow;
   - typography;
   - spacing;
   - component state;
   - a shared primitive.
6. State the smallest proposed visual change.

Do not redesign the page.

## Implementation Rules

- Reuse existing design tokens and shared primitives.
- Match existing breakpoints and responsive conventions.
- Prefer local scoped changes over global CSS.
- Do not replace a component solely because another implementation is easier.
- Do not change API calls, data transformations, trading/business logic, or state ownership for a visual task.
- Preserve accessibility, focus behavior, keyboard navigation, labels, and semantics.
- Preserve loading, error, disabled, and empty states.
- Avoid hard-coded magic numbers when the design system already provides the value.
- If a one-off value is required to match the design, keep it narrowly scoped and explain it.

## Tables / Dense Financial UI

Check:
- column sizing;
- intrinsic minimum widths;
- long symbols/names;
- long numeric values;
- text wrapping/truncation;
- alignment;
- horizontal overflow;
- scrollbar behavior;
- sticky headers/columns;
- tabs/navigation width;
- loading and empty rows;
- small-width behavior;
- semantic colors/indicators.

Do not solve overlap by hiding meaningful content unless the design explicitly requires truncation.

## Figma Matching

When a Figma reference is available:
- compare structure before tweaking individual pixels;
- verify typography, spacing, alignment, borders, radii, and component states;
- preserve established tokens even if raw Figma values differ slightly because of tokenization;
- do not modify areas outside the referenced target.

If Figma conflicts with existing product behavior, report the conflict.

## Responsive Verification

At minimum, reason through or test:
- normal desktop width;
- the width where the issue reproduces;
- a narrower width likely to stress the layout.

Check:
- clipping;
- overlap;
- unexpected wrapping;
- inaccessible controls;
- unwanted page-level horizontal scroll;
- visible/hidden scrollbars;
- content that becomes unreadable.

## Diff Review

Before completion:
- inspect the diff;
- ensure no unrelated style changes;
- ensure no global selector leaked into unrelated screens;
- ensure no business logic changed;
- ensure no mass formatting occurred.

## Completion

Report:
- Visual issue fixed
- Files changed
- Widths/states checked
- Behavior intentionally preserved
- Any remaining Figma mismatch
- Suggested commit message

Stop before commit/push unless explicitly authorized.
