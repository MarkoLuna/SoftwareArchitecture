# Accessibility (a11y)

Web accessibility ensures that people with disabilities can perceive, understand, navigate, and interact with the web. It is both a legal requirement and a quality attribute.

---

## WCAG (Web Content Accessibility Guidelines)

WCAG is organized around four principles (POUR):

| Principle | Description | Examples |
|---|---|---|
| **Perceivable** | Users must be able to perceive the content | Alt text, captions, color contrast |
| **Operable** | Users must be able to operate the interface | Keyboard navigation, enough time, no seizures |
| **Understandable** | Users must be able to understand the content | Clear language, predictable behavior, input assistance |
| **Robust** | Content must work with current and future tools | Semantic HTML, ARIA compatibility |

### Conformance Levels

- **A**: Minimum level (essential support)
- **AA**: Standard level (most legal requirements, including Section 508 and EU directive)
- **AAA**: Highest level (not required for all content)

---

## ARIA (Accessible Rich Internet Applications)

ARIA attributes provide additional semantics for assistive technology when native HTML semantics are insufficient.

### ARIA Rules

- **First rule**: Use native HTML elements when possible (`<button>` over `<div role="button">`)
- **No ARIA is better than bad ARIA**: Incorrect ARIA can make things worse
- **Roles**: Define the type of widget (`role="dialog"`, `role="tablist"`)
- **Properties**: Describe characteristics (`aria-required="true"`, `aria-expanded="false"`)
- **States**: Current conditions (`aria-busy="true"`, `aria-hidden="true"`)

### Common Patterns

```html
<!-- Custom checkbox with ARIA -->
<div
  role="checkbox"
  aria-checked="false"
  tabindex="0"
  @click="toggle"
  @keydown.space.prevent="toggle"
>
</div>

<!-- Live region for dynamic content -->
<div aria-live="polite" aria-atomic="true">
  Cart updated: {{ itemCount }} items
</div>
```

---

## Keyboard Accessibility

All functionality must be operable through a keyboard alone.

- **Tab order**: Logical focus order via DOM order (avoid positive `tabindex` values)
- **Focus indicators**: Visible focus ring (minimum 2px, 3:1 contrast)
- **Skip links**: "Skip to content" link as the first focusable element
- **Roving tabindex**: Manage focus within composite widgets (toolbars, menus, tab panels)

---

## Testing for Accessibility

| Tool | Type | Use |
|---|---|---|
| axe-core | Automated | CI/CD pipeline checks |
| Lighthouse a11y audit | Automated | Quick regression checks |
| WAVE | Browser extension | Visual overlay analysis |
| Screen readers | Manual | NVDA (Windows), VoiceOver (macOS), JAWS |
| Color contrast analyzers | Manual | Check contrast ratios (> 4.5:1 for text) |

---

[⬅️ Back to Frontend Architecture](./README.md)
