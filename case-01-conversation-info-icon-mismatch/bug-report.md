# Bug Report — Conversation Info Icon Mismatch

## Summary

The Conversation Info section in production is rendering a Lucide gauge icon instead of the custom SVG defined in the approved Figma layout. The live icon differs in symbol, proportions, stroke width, and SVG paths, resulting in a visible inconsistency and misalignment with the design system.

## Impact

- Breaks visual alignment between design and implementation
- Reduces recognizability of the Conversation Info affordance
- Introduces an inconsistent iconographic language in the same UI surface
- Signals a potential governance gap between design tokens/components and code

Severity: Medium (UI inconsistency with direct impact on design quality, no data loss or functional breakage).

## Scope

- Affects: Conversation Info section in the conversation details UI
- Platforms: Web (desktop and responsive views)
- Browsers: Reproducible on modern browsers (Chromium- and WebKit-based)
- Users: All users with access to Conversation Info

## Environment

- Environment: Production-like environment
- Feature flags: Standard defaults, no icon-experiment flags enabled

## Steps to Reproduce

1. Open the design file in Figma.
2. Navigate to the Conversation Info section in the layout.
3. Identify the icon defined for the Conversation Info entry.
4. Open the live site in a supported browser.
5. Access any conversation.
6. Locate the Conversation Info section in the live UI.
7. Visually compare the icon rendered in production with the one defined in Figma.

## Expected Result

The icon rendered in the Conversation Info section should match the Figma definition exactly, including:

- Symbol: Same semantic and visual symbol as in the design system
- Proportions: Same width, height, and aspect ratio
- Stroke style: Same stroke width and line caps/joins
- Visual standards: Alignment with the design system’s icon grid and styling

From the Figma reference, the layout specifies:

- Width: 15 px
- Height: 11.719 px

SVG definition:

```svg
<svg xmlns="http://www.w3.org/2000/svg" width="17" height="14" viewBox="0 0 17 14" fill="none">
  <path
    d="M8.33402 11.7183L6.50069 1.0516M7.50004 11.7189C7.50004 11.9399 7.58784 12.1519 7.74412 12.3082C7.9004 12.4644 8.11236 12.5522 8.33337 12.5522C8.55439 12.5522 8.76635 12.4644 8.92263 12.3082C9.07891 12.1519 9.16671 11.9399 9.16671 11.7189C9.16671 11.4979 9.07891 11.2859 8.92263 11.1297C8.76635 10.9734 8.55439 10.8856 8.33337 10.8856C8.11236 10.8856 7.9004 10.9734 7.74412 11.1297C7.58784 11.2859 7.50004 11.4979 7.50004 11.7189ZM0.833374 4.21912L5.41671 8.80245C6.19558 8.03901 7.24274 7.61138 8.33337 7.61138C9.42401 7.61138 10.4712 8.03901 11.25 8.80245L15.8334 4.21912C14.895 3.15505 13.7409 2.30285 12.4478 1.71912C11.1546 1.13539 9.75213 0.833496 8.33337 0.833496C6.91462 0.833496 5.5121 1.13539 4.219 1.71912C2.92589 2.30285 1.77179 3.15505 0.833374 4.21912Z"
    stroke="black"
    stroke-width="1.66667"
    stroke-linecap="round"
    stroke-linejoin="round"
  />
</svg>
```

Style:

- `stroke-width: 1.667px`
- `stroke: var(--base-icon-high, #000)`

The icon should respect the design token for icon color and the specified stroke width.
```

<img width="617" height="427" alt="image" src="https://github.com/user-attachments/assets/fe5117e4-8c61-4129-ad67-5a6a859ff4d8" />


## Actual Result

The live site is using a Lucide gauge icon instead of the custom SVG defined in Figma:

```svg
<svg
  xmlns="http://www.w3.org/2000/svg"
  width="24"
  height="24"
  viewBox="0 0 24 24"
  fill="none"
  stroke="currentColor"
  stroke-width="2"
  stroke-linecap="round"
  stroke-linejoin="round"
  class="lucide lucide-gauge size-4"
  aria-hidden="true"
>
  <path d="m12 14 4-4"></path>
  <path d="M3.34 19a10 10 0 1 1 17.32 0"></path>
</svg>
<img width="409" height="342" alt="image" src="https://github.com/user-attachments/assets/418bf2aa-78ff-4fb6-8956-39ccb8759320" />
```

Observed differences:

- Symbol: Gauge metaphor instead of the custom Conversation Info icon
- Dimensions: 24 × 24 icon instead of the smaller, asymmetric SVG defined in Figma
- Stroke width: `stroke-width="2"` instead of `1.667px`
- Paths: Different geometry and visual weight

This results in a visible UI inconsistency and deviation from the design system.

## Figma Reference

- Design file: Metro Product Design — Latest
- Node: Conversation Info section
- URL: Redacted, refers to a design document containing the approved icon SVG

## Visual Comparison Notes

- The Figma icon is narrower and more compact, with carefully tuned proportions for the Conversation Info row.
- The Lucide gauge icon has a circular, full-grid footprint, which dominates the row visually and breaks alignment with neighboring icons.
- The mismatch is evident both at 1× scale and under high-DPI rendering.

## Suggested Fix (Frontend)

1. Replace the Lucide gauge icon with the approved SVG exported from Figma.
2. Wrap the SVG in a reusable icon component that:
   - Enforces the correct `width`, `height`, and `viewBox`.
   - Binds `stroke` to the design system token (for example, `var(--base-icon-high)`).
   - Uses the exact `stroke-width` specified in the design file.
3. Ensure the icon is integrated into the existing design system/icon library rather than inlined ad hoc.
4. Add a visual regression check or Storybook story for the Conversation Info state to avoid future regressions.

## Acceptance Criteria

- The Conversation Info icon in production visually matches the approved Figma SVG at 1× and 2× scales.
- The icon uses the correct design tokens for color and stroke width.
- No other icons in the Conversation Info area are changed unintentionally.
- QA sign-off confirms that Figma vs. production comparison passes on supported browsers.

