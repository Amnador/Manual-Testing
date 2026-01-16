# Case 03 — Input Text Selection Inconsistency

This case study documents inconsistent text selection behavior in input fields compared to non-editable text, driven by conflicting Tailwind utilities and borderline visual states during selection.

The main focus areas are:
- Different selection colors for editable inputs vs. static text
- A visible border appearing during selection in specific configurations
- Conflicting utility classes (`selection:*`, `border` vs. `border-none`, `ring` vs. `ring-0`)
- A temporary frontend-only fix validated in a production-like environment

The goal is to provide a clear, actionable description suitable for a senior-level QA and frontend portfolio, while explicitly flagging open questions for Design/UX regarding border presence during selection.

