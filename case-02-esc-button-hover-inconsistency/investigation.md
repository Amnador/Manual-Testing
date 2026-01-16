# Investigation — Esc Button Hover Inconsistency

## Objective

Understand why User and Organization Esc-style filter buttons exhibit inconsistent hover states, and validate whether the problem originates in missing design specifications, implementation choices, or both.

## Methodology

- Compare the Figma layout against the live UI for both filters.
- Inspect the DOM and applied CSS/Tailwind utilities for each button.
- Analyze theme handling (light and dark mode) for hover states.
- Identify whether filters share a common component or use divergent implementations.

## Steps Performed

1. Opened the relevant filter bar screen in Figma.
2. Located the User filter and Organization filter elements.
3. Checked for explicit hover state definitions:
   - Separate frames or variants for `hover`.
   - Tokens or annotations referencing hover background and text colors.
4. Opened the live UI in a browser and navigated to the filter bar.
5. With theme set to light:
   - Hovered the User filter and inspected styles via devtools.
   - Hovered the Organization filter and compared the computed styles.
6. Repeated the same inspection with theme set to dark.
7. Compared the underlying implementation:
   - Whether both filters use the same React/Vue/SPA component or separate instances.
   - Which Tailwind utility classes are bound to each button.

## Findings

- Figma does not define explicit hover variants for these filters:
  - The main layout specifies base typography, spacing, and alignment.
  - There is no dedicated documentation on hover behavior for Esc-style filters.
- In the live UI:
  - User and Organization filters share a similar visual role but do not share the exact same hover styling.
  - Tailwind class combinations differ between the two, leading to:
    - Different background colors on hover.
    - Different text color behavior on hover.
- Theme handling:
  - In light mode, at least one filter produces a hover state that reduces contrast.
  - In dark mode, hover colors are not mirrored cleanly; one filter may invert text/background, while the other does not.

These observations indicate that the inconsistency is primarily caused by implementation decisions in the absence of explicit design guidance, not by backend logic.

## Frontend vs. Backend Assessment

- Filter options and labels are data-driven, but visual styling is purely frontend.
- No API response fields dictate hover color or theme behavior.
- The inconsistency is isolated to CSS/Tailwind usage and component structure.

Conclusion: This is a frontend/UI issue caused by divergent Tailwind class combinations and lack of a shared Esc-style filter component.

## Recommended Follow-Up

- Align with Design/UX on a minimal, high-contrast hover specification for Esc-style filters in both light and dark modes.
- Capture this behavior as:
  - A documented variant in the design system.
  - A shared reusable component in the frontend codebase.
- Consolidate filter implementations so that User and Organization filters use the same component and class set.

