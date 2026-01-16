# Hypotheses — Esc Button Hover Inconsistency

## Likely Root Causes

1. **Missing hover definition in Figma**
   - The design file only specifies the resting state for the filters.
   - Hover behavior was left undefined, forcing engineers to make local decisions.

2. **Divergent Tailwind class usage**
   - User and Organization filters were implemented separately.
   - Different combinations of text and background utilities were used for hover.
   - Theme variants (light vs. dark) were not normalized across both filters.

3. **No shared Esc-style filter component**
   - There is no single design-system component representing an Esc-style filter.
   - Teams implemented similar buttons as ad hoc instances, leading to drift.

## Secondary Contributing Factors

- Limited documentation on theme-specific behavior for small filter buttons.
- Inconsistent review focus on interaction states (hover/active/focus) vs. static layouts.

## Regression Risks

1. **New filters reusing inconsistent patterns**
   - Additional filters added to the bar may copy one of the existing inconsistent implementations.
   - Over time, the filter bar could accumulate multiple hover behaviors.

2. **Theme changes amplifying contrast issues**
   - Future adjustments to light/dark palettes could reduce contrast further where hover colors are not based on tokens.

## Mitigation Strategy

- Define a canonical hover behavior for Esc-style filters in collaboration with Design/UX.
- Implement a shared Tailwind-based recipe:
  - Light mode: `text-xs text-white hover:bg-black`
  - Dark mode: `text-xs text-black hover:bg-white`
- Wrap this recipe in a reusable component and apply it consistently to:
  - User filter
  - Organization filter
  - Any future filter buttons using the same pattern
- Add simple UI tests or visual checks for hover states across themes to detect regressions.

