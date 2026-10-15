---
name: responsive-css
description: Create or revise maintainable, responsive CSS for web pages and components across viewport sizes, zoom levels, input modes, and content lengths. Use when layout or visual styling changes; preserve the existing design system and solve cascade problems without !important or specificity hacks.
---

# Responsive CSS

Build resilient layouts that adapt to the available space, user preferences, real content, and established project design. Prefer the smallest change consistent with the existing design system and browser-support policy.

## Work With the Cascade

Never use `!important`, including in new utilities or one-off overrides. Do not replace it with inline styles, IDs used for styling, repeated selectors, excessive nesting, or artificially high specificity.

- Keep selectors class-based and low-specificity where practical. Use `:where()` when a deliberately zero-specificity wrapper makes component rules easier to override.
- Order rules by a clear component, layer, or stylesheet policy. Use cascade layers only when they fit the project's architecture and browser support.
- Resolve conflicts at their source: remove accidental specificity, narrow the component boundary, place the rule in the correct layer, or expose an explicit variant.
- Reuse the project's custom properties, tokens, utilities, and component variants. Introduce a new token only when the value represents a repeated design decision.
- Keep state styling explicit with classes, attributes, or semantic pseudo-classes. Avoid selectors coupled to fragile DOM depth.
- Do not change global resets or broad element rules to solve one local component defect.

## Design for Available Space

- Start with the narrow layout and add enhancements when content needs more space. Choose breakpoints from actual layout failures rather than named devices.
- Prefer normal flow, flexbox, and grid over fixed positioning or manual coordinates.
- Use fluid sizing with relative units, `min()`, `max()`, `clamp()`, `minmax()`, and intrinsic layout where they make the component more resilient.
- Use container queries for reusable components whose layout depends on their container, when the project supports them. Use media queries for viewport and user preferences.
- Avoid fixed widths and heights for text containers. Allow text to wrap, controls to grow, and translated or user-generated content to fit.
- Prevent accidental horizontal scrolling at narrow widths and high zoom. Do not hide overflowing content merely to conceal a broken layout.
- Keep media responsive with constrained inline size and preserved aspect ratio. Reserve space for asynchronously loaded media when layout shift matters.
- Use logical properties when they improve writing-mode or direction support without fighting established project conventions.

## Preserve Usability and Accessibility

- Do not remove focus indicators. Style `:focus-visible` clearly and ensure focus is not obscured by sticky or fixed content.
- Maintain sufficient text and non-text contrast in every interactive state, including hover, focus, active, disabled, validation, and selected states.
- Do not communicate meaning through color alone. Pair it with text, shape, iconography, or another persistent cue.
- Keep interactive targets usable for touch and pointer input, with adequate size and spacing for the interface context.
- Use hover styles as an enhancement, not the only way to reveal essential content or controls.
- Respect `prefers-reduced-motion` for non-essential animation and avoid motion, flashing, or parallax that can make the interface unusable.
- Do not disable text selection, browser zoom, or user font scaling.
- Use visually hidden text only with a proven accessible pattern; do not hide focusable content off-screen in a way that creates unexpected focus movement.

## Keep Components Stable

- Scope styles to the owning component and define intentional variants instead of accumulating page-specific overrides.
- Test with short, long, missing, localized, and user-generated content where those states are possible.
- Keep responsive behavior understandable: avoid many narrowly spaced breakpoints that repeatedly undo earlier declarations.
- Preserve useful browser defaults unless the design explicitly replaces them with an equally usable alternative.
- Avoid browser-specific workarounds without a reproduced defect and a documented reason consistent with the support policy.

## Verify

Inspect the rendered page or component at the project's supported sizes and states. At minimum:

1. test narrow mobile, intermediate, and wide layouts at the widths where the content changes behavior;
2. zoom text to 200% and verify readability, focus visibility, and access to every control without loss of content or function;
3. verify reflow at 320 CSS pixels wide without two-dimensional scrolling for ordinary page content;
4. test keyboard focus, hover-independent operation, reduced motion, long content, and relevant validation or error states;
5. check for overflow, overlap, clipped text, layout shift, undersized targets, and unexpected cascade conflicts;
6. run the project's CSS lint, build, visual regression, or browser checks when available.

Search the changed CSS for `!important` before completion. If existing unrelated uses remain, do not expand the task into a broad rewrite; ensure the new change does not add or depend on them and report any existing rule that blocks a clean implementation.
