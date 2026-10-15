---
name: accessible-html5
description: Create or revise semantic, accessible HTML5 for web interfaces, templates, forms, navigation, tables, media, and dynamic UI. Use when markup or user interaction changes; prefer native HTML semantics and add ARIA only when native elements cannot express the required role, state, or relationship.
---

# Accessible HTML5

Build markup whose structure, meaning, and interaction work with browsers, keyboards, screen readers, zoom, and other assistive technology. Preserve the project's template language and component conventions while keeping the rendered HTML valid.

## Use Native HTML First

- Use the element that matches the content or action: links navigate, buttons perform actions, headings define sections, and form controls collect input.
- Use landmarks such as `header`, `nav`, `main`, `aside`, and `footer` when they describe the page structure. Keep one primary `main` region and give repeated landmarks an accessible name.
- Maintain a logical heading hierarchy that reflects the document outline. Do not choose heading levels for their visual size.
- Use lists, tables, `figure`, `details`, dialog elements, and other native structures when their built-in semantics and behavior fit the content.
- Keep DOM order meaningful and aligned with reading and keyboard order. Do not use CSS positioning or positive `tabindex` values to repair an illogical source order.
- Give the document an appropriate language and each page a descriptive, context-specific title.

Do not recreate a native control with a generic `div` or `span`. When a custom widget is genuinely required, implement its complete keyboard interaction, focus behavior, role, states, and accessible name according to the relevant ARIA pattern.

## Apply ARIA Deliberately

Native HTML semantics take precedence over ARIA. Do not add a role that merely repeats an element's implicit role, and never use ARIA to compensate for an invalid or non-interactive element.

- Ensure every interactive control has an accessible name derived from visible text, a label, or—only when needed—an ARIA naming attribute.
- Use `aria-labelledby` and `aria-describedby` to express real relationships. Every referenced ID must exist and remain unique in the rendered page.
- Reflect dynamic state with the applicable attribute, such as `aria-expanded`, `aria-current`, `aria-selected`, `aria-pressed`, or `aria-invalid`, and update it at the same time as the visible state.
- Use live regions sparingly for asynchronous information that users would otherwise miss. Do not announce ordinary static content or repeatedly interrupt the user.
- Never hide focusable or essential content from assistive technology with `aria-hidden="true"`.
- Do not add `tabindex` values greater than zero. Use `tabindex="-1"` only for intentional programmatic focus targets.

ARIA changes the accessibility tree, not the element's behavior. Supply the required keyboard and focus logic separately.

## Forms and Feedback

- Associate every control with a persistent visible `label`, except where the control's purpose is already unambiguous from accessible content.
- Group related controls with `fieldset` and `legend` where that relationship matters.
- Use appropriate input types, `name`, `autocomplete`, and input-purpose metadata. Treat placeholders as examples or hints, not replacements for labels.
- Identify required fields and instructions in text as well as programmatically.
- Connect help text and validation messages to their controls. On failed submission, provide an error summary and move focus only when that improves recovery without losing user context.
- Preserve entered values after validation failure and make errors understandable without relying on color, position, or an icon alone.

## Content and Media

- Give informative images concise alternative text. Use empty `alt` text for decorative images and avoid duplicating adjacent captions.
- Give complex images an equivalent explanation in nearby content when a short alternative cannot convey their purpose.
- Provide captions or transcripts for time-based media when required by the content, and do not autoplay media with sound.
- Mark data tables with real headers and associate complex header structures explicitly. Do not use tables for visual layout.
- Write link and control text that communicates its purpose in context; avoid repeated ambiguous labels such as “click here”.

## Dynamic Interfaces

- Support all functionality from a keyboard without traps. Keep focus visible and restore or move it predictably after dialogs, menus, inserted content, and route changes.
- Prefer progressive enhancement: essential content and actions should remain understandable when optional client-side behavior fails.
- Provide a way to bypass repeated navigation when a page has substantial repeated content.
- Preserve semantic and accessible output across loading, empty, success, validation, and error states.

## Verify

Inspect the rendered DOM, not only the source template. For affected pages or components:

1. validate HTML and check for duplicate IDs, broken references, invalid nesting, and inaccessible names;
2. operate the complete interaction with a keyboard and confirm logical focus order, visible focus, and no keyboard trap;
3. inspect the accessibility tree or run the project's established automated accessibility checker;
4. test important form, loading, empty, error, and success states;
5. perform a brief manual screen-reader check for new or substantially changed custom interaction when suitable tooling is available.

Automated checks supplement manual keyboard and semantic review; they do not prove accessibility by themselves.
