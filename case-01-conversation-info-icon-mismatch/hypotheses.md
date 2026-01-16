# Hypotheses — Conversation Info Icon Mismatch

## Likely Root Causes

1. **Fallback to a generic library icon**
   - The Conversation Info component was implemented before the custom SVG was finalized, or without access to it.
   - A generic Lucide gauge icon was selected as a temporary substitute and never updated.

2. **Missing design-system binding**
   - The Conversation Info icon is not registered as a first-class icon in the design system.
   - Engineers reached for the icon library directly instead of a design-system-provided component, bypassing visual guidelines.

3. **Inconsistent specs between Figma and implementation**
   - Figma specifies precise dimensions and stroke properties, but these specifications were not captured in engineering documentation or component APIs.
   - Icon size defaults from the library (`24 × 24`, `stroke-width="2"`) were used instead.

4. **Lack of visual regression coverage**
   - There are no screenshots, Storybook stories, or automated checks asserting the exact icon used in the Conversation Info section.
   - This allowed the mismatch to ship and remain unnoticed.

## Secondary Contributing Factors

- Absence of a centralized mapping between semantic icon names (for example, `conversationInfo`) and actual SVG assets.
- Limited cross-check between implementation and Figma during code review for this specific surface.

## Regression Risks

1. **Future icon swaps**
   - If the icon remains sourced directly from a generic icon library, future refactors or library upgrades might silently change the icon again.

2. **Inconsistent iconography across features**
   - Other teams may replicate the pattern of using generic icons instead of design-system-approved assets, leading to divergent icon styles within the same product.

3. **Theming and token drift**
   - If the SVG does not bind to design tokens for stroke and color, theme changes (for example, high contrast or dark mode) may not correctly propagate to this icon.

## Mitigation Strategy

- Introduce a dedicated Conversation Info icon in the design system/icon library and enforce its usage.
- Document the icon specifications (dimensions, `viewBox`, stroke properties, tokens) alongside the component.
- Add a visual regression test or Storybook screenshot test for the Conversation Info surface.
- During code review, explicitly compare critical UI surfaces (such as Conversation Info) against Figma before sign-off.

