# Investigation — Conversation Info Icon Mismatch

## Objective

Confirm whether the icon discrepancy between Figma and the live Conversation Info section is caused by frontend implementation, and assess the scope of the inconsistency across the UI.

## Methodology

- Perform a side-by-side comparison of the Figma design and the live UI.
- Inspect the DOM and SVG implementation in the browser.
- Validate whether the icon is sourced from a design-system component, an icon library, or a hardcoded SVG.
- Check for any design documentation or component specs related to the Conversation Info icon.

## Steps Performed

1. Opened the Figma file containing the Conversation Info section.
2. Located the Conversation Info row and inspected the icon component.
3. Verified:
   - Symbol and semantics of the icon.
   - Exact `width`, `height`, and `viewBox`.
   - Stroke properties (`stroke-width`, `stroke-linecap`, `stroke-linejoin`).
4. Opened the production-like environment in the browser and navigated to any conversation.
5. Inspected the Conversation Info section using browser devtools.
6. Identified the rendered SVG and associated CSS classes.
7. Compared:
   - SVG tag attributes (dimensions, `viewBox`, stroke).
   - Path definition and geometry.
   - Icon source (Lucide library vs. custom SVG asset).

## Findings

- Figma defines a custom SVG for the Conversation Info icon with:
  - Asymmetric dimensions (approximately 15 × 11.719).
  - Stroke width of `1.667px`.
  - Custom path geometry tailored for the Conversation Info use case.
- The live UI renders an icon with:
  - Dimensions of `24 × 24`.
  - Stroke width of `2`.
  - `class="lucide lucide-gauge size-4"`, indicating usage of a generic Lucide gauge icon.
- The icon in code is not mapped to a design-system-specific Conversation Info token or component; it is instantiated directly from the icon library.

## Frontend vs. Backend Assessment

- No backend response fields or API data influence which icon is rendered; the icon choice is static in the UI layer.
- There is no evidence of feature flags controlling icon variants.
- The discrepancy is fully attributable to frontend implementation (component/icon selection and SVG configuration).

Conclusion: This is a frontend-only issue caused by using a generic Lucide icon instead of the custom design-system SVG.

## Additional Checks

- Searched for the Lucide gauge icon usage in the codebase to identify shared components.
- Verified that other icons in the same area follow design-system patterns more closely.
- No other immediate mismatches were found in the Conversation Info row, but the presence of this mismatch suggests a potential gap in icon governance.

## Recommended Follow-Up

- Introduce a dedicated Conversation Info icon in the design system/icon library.
- Replace direct Lucide icon usage with a design-system-wrapped component that:
  - Encapsulates the approved SVG.
  - Applies the correct dimensions and tokens by default.
- Align design and engineering on the source of truth for iconography in this surface.

