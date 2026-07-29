# Web Components

Web Components are a set of browser-native APIs for creating reusable, encapsulated, framework-agnostic custom elements. They work across all modern browsers without any library or framework.

---

## Core Technologies

### Custom Elements

Define new HTML elements with custom behavior using JavaScript classes.

```js
class UserCard extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
  }

  connectedCallback() {
    const name = this.getAttribute('name') || 'User';
    this.shadowRoot.innerHTML = `<div class="card"><h2>${name}</h2></div>`;
  }
}

customElements.define('user-card', UserCard);
```

- **Lifecycle callbacks**: `connectedCallback`, `disconnectedCallback`, `attributeChangedCallback`, `adoptedCallback`
- **Observed attributes**: `static get observedAttributes()` for attribute change reactions
- **Element types**: Autonomous elements (extends `HTMLElement`) or customized built-ins (extends specific HTML elements)

### Shadow DOM

Encapsulation boundary that isolates the component's DOM, styles, and events from the rest of the page.

- **`mode: 'open'`**: Shadow root accessible via `element.shadowRoot` (testable, inspectable)
- **`mode: 'closed'`**: Shadow root inaccessible from outside (rarely used — breaks tooling and testing)
- **Style scoping**: Styles inside the shadow tree don't leak out; page styles don't leak in
- **Slots**: Project content from the light DOM into the shadow tree (`<slot name="header">`)

### HTML Templates

Define fragments of HTML that are parsed but not rendered until instantiated.

```html
<template id="user-card-template">
  <style>.card { border: 1px solid #ccc; padding: 1rem; }</style>
  <div class="card">
    <slot name="avatar"></slot>
    <h2><slot name="name">User</slot></h2>
  </div>
</template>
```

### ES Modules

Web Components are typically distributed as ES modules, enabling native import without a bundler.

```js
import './user-card.js';
```

---

## Frameworks and Web Components

| Framework | Web Component Support |
|---|---|
| **Lit** | First-class — Google's library for building Web Components with reactive properties and templating |
| **React** | Good — use `ref` and event listeners; React 19 improved custom element support |
| **Vue** | Excellent — native support with `defineCustomElement()` |
| **Angular** | Good — `createCustomElement()` wraps Angular components as Custom Elements |
| **Svelte** | Excellent — `customElement: true` option compiles to a Custom Element |

---

[⬅️ Back to Frontend Architecture](./README.md)
