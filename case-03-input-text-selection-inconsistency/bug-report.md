# Bug Report — Input Text Selection Inconsistency

## Summary

When users select text inside input fields, the visual selection behavior differs from both:
- The selection styling specified via `selection:bg-primary` and `selection:text-primary-foreground`, and
- The selection behavior for non-editable text on the same page.

Additionally, a border appears around the input during selection under certain configurations, even when the intended design is a borderless input. Conflicting Tailwind utilities (for example, `border` with `border-none`, `ring` with `ring-0`) contribute to inconsistent and sometimes surprising behavior.

## Impact

- Users experience different selection feedback depending on whether text is editable or static.
- Visual noise is introduced by an unexpected border appearing only while text is selected.
- The inconsistency complicates QA, as selection behavior is not predictable across components.
- It suggests a misalignment between Tailwind utility usage and design system expectations.

Severity: Low to Medium (UX inconsistency with some visual distraction, no data loss).

## Scope

- Affects: Text input fields where Tailwind `selection:*` utilities and border/ring utilities are combined.
- Surfaces: Forms, filter inputs, or search fields using the same base component.
- Comparison baseline: Non-editable text blocks on the same screen.

## Environment

- Environment: Production-like environment
- CSS stack: Tailwind CSS with component abstractions
- Themes: Applies to both light and dark mode; exact perception varies with palette

## Steps to Reproduce

1. Open a page containing:
   - At least one text input using Tailwind `selection:bg-primary` and `selection:text-primary-foreground`.
   - Nearby static text (for example, a label or paragraph) also using the same selection utilities, or inheriting defaults.
2. In a supported browser, click into the input field.
3. Type any text (for example, “test selection behavior”).
4. Select part or all of the text inside the input using mouse drag or keyboard shortcuts.
5. Observe:
   - Background and text color during selection.
   - Whether a border or ring appears only while the selection is active.
6. Select text in a non-editable block (for example, a span or paragraph) on the same page.
7. Compare the selection color and border behavior between:
   - Selected input text
   - Selected static text

## Expected Result

- Selection styling should be consistent and intentional:
  - `selection:bg-primary` and `selection:text-primary-foreground` should apply coherently to editable and non-editable text, or there should be a clearly documented difference.
  - Inputs intended to appear borderless should not show a transient border only during text selection.
  - Border and ring utilities should not conflict at runtime; the visual hierarchy should be stable across focus, hover, and selection states.

## Actual Result

- When selecting text inside the input:
  - The selection background and text color do not always match the selection behavior of nearby static text.
  - A border becomes visible during selection in some states, even when the default visual is borderless.
- Inspection shows that multiple Tailwind utilities are being applied simultaneously:
  - `selection:bg-primary` and `selection:text-primary-foreground`
  - Border utilities such as `border` and `border-none`
  - Ring utilities such as `ring` and `ring-0`
- The combination leads to:
  - Inputs and static text using different fallback selection styles.
  - Border and ring behavior that changes only when selection is active, producing a “jumping” outline effect.

## Temporary Fix

A temporary mitigation was validated:

- Simplify the Tailwind class set applied to the input:
  - Remove conflicting border definitions (for example, avoid using both `border` and `border-none`).
  - Normalize ring usage so that only one of `ring` or `ring-0` applies in the relevant state.
  - Ensure `selection:bg-primary` and `selection:text-primary-foreground` are applied consistently to the input text.
- After this simplification:
  - The unexpected border on selection no longer appears.
  - Selection behavior between input text and nearby static text is closer to the intended design.

This is a frontend-only fix and does not require backend changes.

## Open Questions for Design/UX

- Should input fields display any border or outline when text is selected, even if the resting state is borderless?
- Is the selection styling for input text meant to:
  - Match non-editable text exactly, or
  - Use a distinct but consistent pattern (for example, slightly different background or text color)?
- Should the design system define explicit guidance for:
  - Selection states across inputs, textareas, and static text
  - Interaction between border, ring, and selection for focus versus selection

These questions should be resolved to avoid future divergence between implementations.

## Acceptance Criteria

- Input text selection behavior is consistent with the agreed design system specification.
- No unexpected border appears only during selection for inputs meant to be borderless.
- Tailwind utilities applied to input components are conflict-free and stable across hover, focus, and selection states.

