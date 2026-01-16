# Bug Report — Esc Button Hover Inconsistency

## Summary

The hover behavior for Esc-style filter buttons is inconsistent between the User filter and the Organization filter. The current implementation produces different visual feedback for buttons that should be visually and behaviorally aligned, and in some cases degrades text legibility depending on the active theme.

The design file does not provide a clear hover specification for these buttons, leaving implementation choices to engineers and leading to divergent behavior.

## Impact

- Users receive inconsistent visual feedback for similar actions in the same filter bar.
- In certain theme combinations, hover colors reduce text contrast and legibility.
- The mismatch erodes trust in the design system and in the predictability of filters.

Severity: Medium (no functional failure, but clear UX and consistency issues).

## Scope

- Affects: Esc-like filter buttons in the filter bar (for example, User and Organization filters).
- Surfaces: Web UI filter bar or toolbar where these filters appear.
- Themes: Light and dark modes.

## Environment

- Environment: Production-like environment with theme switching enabled.
- Modes: Light mode and dark mode.

## Steps to Reproduce

1. Open the application in a supported browser.
2. Ensure the filter bar containing User and Organization filters is visible.
3. In light mode:
   1. Hover over the User filter button.
   2. Observe background, text color, and border behavior.
   3. Hover over the Organization filter button.
   4. Compare the visual hover feedback with the User filter.
4. Switch to dark mode.
5. Repeat the hover interactions on both User and Organization filter buttons.
6. Observe whether hover behavior and legibility remain aligned between both filters.

## Expected Result

- User and Organization filters should:
  - Share the same base visual tokens (typography, border, padding).
  - Use a consistent hover pattern for background and text color.
  - Preserve minimum contrast ratios for legibility in both light and dark modes.
- Hover behavior should be clearly defined in the design system or in Figma so that:
  - Engineers do not need to guess hover states.
  - The same component behaves identically wherever used.

Suggested hover model (Tailwind classes) for these Esc-style buttons:

- Light mode:
  - `text-xs text-white hover:bg-black`
- Dark mode:
  - `text-xs text-black hover:bg-white`

The final implementation can adapt these classes using theme-aware variants, but the intent is:
- Compact text (`text-xs`)
- High-contrast text color versus background on hover
- Symmetric behavior between light and dark themes

## Actual Result

- Hover states differ between User and Organization filters:
  - One filter may invert colors or apply a different background on hover.
  - The other may keep a subtle or no hover feedback.
- In at least one theme:
  - Hover background and text color combination approaches a low-contrast state.
  - The text becomes harder to read at a glance.
- There is no single, shared “Esc-style filter” hover pattern applied across the filter bar.

## Figma Reference

- Figma file shows the User and Organization filters but does not explicitly define:
  - Hover background color
  - Hover text color
  - Theme-specific behavior (light vs. dark mode)
- As a result, hover states were inferred and implemented differently.

## Suggested Fix (Frontend)

1. Introduce a shared Esc-style filter button variant in the design system components.
2. Implement hover behavior using theme-aware Tailwind utilities:
   - Base:
     - `text-xs`
   - Light mode:
     - `text-white hover:bg-black`
   - Dark mode:
     - `text-black hover:bg-white`
3. Ensure that:
   - The same component is reused for both User and Organization filters.
   - No ad hoc overrides are applied at the call site for hover behavior.
4. Validate hover states in both light and dark modes for:
   - Legibility (text vs. background contrast)
   - Consistency across all Esc-style filters.

## Acceptance Criteria

- Hover behavior for User and Organization filters is visually identical in both light and dark modes.
- Text remains clearly legible on hover for both themes.
- Figma and implementation reach an agreed, documented hover specification.
- QA verifies behavior in the main supported browsers.

