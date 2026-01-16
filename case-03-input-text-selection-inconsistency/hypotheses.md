# Hypotheses — Input Text Selection Inconsistency

## Likely Root Causes

1. **Conflicting border utilities**
   - Inputs are styled with both `border` and `border-none` in different variants.
   - Under certain states (focus + selection), the active combination reveals a border that is otherwise hidden.

2. **Conflicting ring utilities**
   - Inputs use both `ring` and `ring-0` across states.
   - Selection interacts with focus styles, causing ring visibility to change unexpectedly.

3. **Differences between input and static text selection handling**
   - Browsers treat text selection in inputs differently from `::selection` on static text.
   - Custom `selection:bg-primary` and `selection:text-primary-foreground` styles may be applied inconsistently across these elements if not carefully scoped.

4. **Insufficient design-system guidance**
   - The design system specifies general colors and input styles but does not clearly define:
     - How selection should look inside inputs vs. static text.
     - Whether borderless inputs should ever show a border during selection.

## Secondary Contributing Factors

- Incremental addition of utilities over time (for example, adding `ring` for focus without revisiting border behavior).
- Lack of automated tests or visual specs focusing on selection state.

## Regression Risks

1. **New input variants reintroducing conflicts**
   - New components may repeat the pattern of mixing `border`, `border-none`, `ring`, and `ring-0` without a clear policy.

2. **Theming adjustments changing perceived selection contrast**
   - Palette changes may cause selection colors that were previously acceptable to become low-contrast or visually noisy, especially in inputs.

## Mitigation Strategy

- Establish a clear input selection specification with Design/UX:
  - Define whether inputs should show a border or ring during selection.
  - Align on selection background and text colors for inputs vs. static text.
- Refactor input components to:
  - Use a single, non-conflicting set of border utilities.
  - Use a consistent ring strategy (for example, ring only on focus, not on selection).
  - Apply `selection:bg-primary` and `selection:text-primary-foreground` in a predictable, documented way.
- Add visual regression tests or Storybook stories demonstrating:
  - Rest, hover, focus, and selection states for inputs and static text across themes.

