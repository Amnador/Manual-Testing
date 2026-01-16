# Investigation — Input Text Selection Inconsistency

## Objective

Identify why input text selection behaves differently from non-editable text selection and why a border appears during selection in specific input configurations, focusing on Tailwind utility interactions.

## Methodology

- Compare input vs. static text selection behavior in the same UI.
- Inspect applied Tailwind utilities on input components and static text containers.
- Validate how browser default selection behavior interacts with `selection:*` utilities.
- Experiment with removing or adjusting conflicting utilities to confirm root causes.

## Steps Performed

1. Located a page containing:
   - At least one text input using `selection:bg-primary` and `selection:text-primary-foreground`.
   - Nearby non-editable text content.
2. Used devtools to inspect:
   - The exact Tailwind classes applied to the input wrapper and input element.
   - The classes (if any) applied to the nearby static text elements.
3. Performed controlled interactions:
   - Focused the input, typed text, and selected it with mouse and keyboard.
   - Selected text in the static text elements.
4. Observed the following during selection:
   - Background and foreground colors.
   - Presence of border or outline changes.
   - Any ring-related visual effects (for example, box-shadow).
5. Iteratively toggled Tailwind utilities in devtools:
   - Removed `border` vs. `border-none` to see which one took effect.
   - Toggled `ring` vs. `ring-0`.
   - Ensured `selection:bg-primary` and `selection:text-primary-foreground` remained applied.

## Findings

- Inputs and non-editable text do not share exactly the same CSS context:
  - Inputs are subject to browser default focus and selection styles.
  - Static text relies mostly on `::selection` styling.
- In the inspected inputs:
  - Both `border` and `border-none` were present, relying on ordering and specificity to decide the final style.
  - Both `ring` and `ring-0` were present in different state variants (focus, error, etc.).
  - This created subtle differences between resting, focus, and selection states.
- When text was selected inside the input:
  - A border became visible in some combinations, especially when `ring` or focus styles were active.
  - The selection color for input text was sometimes influenced by browser defaults or component-specific overrides, diverging from `selection:bg-primary` and `selection:text-primary-foreground` as seen on static text.

After simplifying the applied utilities to remove conflicts, the selection behavior stabilized:
- Inputs no longer showed an unexpected border only during selection.
- Selection colors were more predictable and closer to the design intent.

## Frontend vs. Backend Assessment

- The behavior is fully driven by frontend styling and browser defaults.
- No API responses or backend configuration influence selection colors or border behavior.

Conclusion: The inconsistency is a frontend-only issue, caused by conflicting Tailwind utilities (border and ring) interacting with browser selection and focus handling.

## Recommended Follow-Up

- Define a canonical selection behavior for:
  - Inputs and textareas
  - Static text content
- Encode this behavior into the design system and component library:
  - Use a consistent set of `selection:*` utilities.
  - Avoid combining mutually exclusive utilities such as `border` and `border-none`, or `ring` and `ring-0` without a clear precedence model.
- Add documentation for how borders and rings should behave during:
  - Resting state
  - Hover
  - Focus
  - Selection

