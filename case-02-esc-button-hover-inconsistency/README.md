# Case 02 — Esc Button Hover Inconsistency

This case study describes a hover-state inconsistency affecting Esc-like filter buttons in a filter bar, with a focus on the User filter and Organization filter.

The core of the issue is:
- Inconsistent hover states between two visually similar buttons
- Lack of explicit hover specification in Figma for these controls
- Reduced legibility in one or more themes due to color combinations
- Misalignment with expected behavior for light and dark mode

The goal is to propose a simple, consistent hover model using Tailwind utility classes that is safe for both themes and easy to implement in the frontend.

