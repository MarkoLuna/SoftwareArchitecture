# CSS Architecture

Organizing CSS at scale requires a deliberate strategy to avoid specificity wars, unused styles, and maintenance nightmares. Modern CSS architecture combines naming conventions, encapsulation techniques, and utility-first frameworks.

---

## Approaches

### Utility-First (Tailwind CSS)

Provides low-level utility classes (`p-4`, `text-center`, `flex`) that are composed directly in HTML. No context switching between HTML and CSS files.

- **Pros**: Rapid prototyping, consistent design system, tiny production CSS (purging unused utilities)
- **Cons**: Verbose HTML, learning curve for utility naming, can feel like inline styles
- **Best for**: Teams that want consistent design tokens without writing custom CSS

### CSS Modules

Scopes CSS to a single component by generating unique class names at build time. Each React/Vue component gets its own `.module.css` file.

- **Pros**: True scoping, no class name collisions, vanilla CSS syntax
- **Cons**: Dynamic styles require workarounds, global styles still need separate handling
- **Best for**: Component-based SPAs where encapsulation matters

### CSS-in-JS (styled-components, Emotion)

Styles are written in JavaScript/TypeScript and injected at runtime or extracted at build time.

- **Pros**: Dynamic styles based on props, co-located with component logic, automatic critical CSS
- **Cons**: Runtime overhead (unless using compiled alternatives like Linaria or Vanilla Extract), larger bundle
- **Best for**: React applications with heavy dynamic theming

### BEM (Block Element Modifier)

A naming convention: `.block__element--modifier`. No tooling required — pure naming discipline.

- **Pros**: Framework-agnostic, predictable specificity, no tooling
- **Cons**: Manual discipline required, long class names, can feel verbose
- **Best for**: Teams wanting a lightweight, tooling-free approach

---

## Design Tokens

Design tokens are the atoms of a design system — named values for colors, spacing, typography, shadows, and breakpoints. They bridge the gap between design (Figma) and code.

```json
{
  "color": {
    "primary": { "500": "#3b82f6" },
    "neutral": { "100": "#f5f5f5", "900": "#171717" }
  },
  "spacing": { "xs": "4px", "sm": "8px", "md": "16px", "lg": "24px" },
  "font": { "sans": "Inter, system-ui, sans-serif" }
}
```

Tokens can be consumed by any CSS approach — Tailwind (config), CSS custom properties, or JavaScript theme objects.

---

## Comparison

| Approach | Scoping | Dynamic Styles | Bundle Size | Learning Curve |
|---|---|---|---|---|
| Tailwind CSS | Global (by convention) | Limited (arbitrary values) | Tiny (purged) | Medium |
| CSS Modules | Component-scoped | Limited (custom properties) | Small | Low |
| CSS-in-JS | Component-scoped | Full (props-based) | Medium-High | Low |
| BEM | Global (naming convention) | Manual | Zero | Low |

---

[⬅️ Back to Frontend Architecture](./README.md)
