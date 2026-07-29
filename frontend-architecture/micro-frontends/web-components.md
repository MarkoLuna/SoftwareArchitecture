# Web Components (Micro-frontends)

## Architecture Overview

```mermaid
graph TB
    subgraph MainApp[Main Application]
        ShellApp[Shell Application]
        ComponentRegistry[Component Registry]
        ShadowDOM[Shadow DOM]
    end
    
    subgraph MicroFrontends[Micro-frontend Applications]
        MFE1[Micro-frontend 1]
        MFE2[Micro-frontend 2]
        MFE3[Micro-frontend 3]
    end
    
    subgraph Components[Web Components]
        Button[Button Component]
        Card[Card Component]
        Modal[Modal Component]
        Form[Form Component]
    end
    
    subgraph Integration[Integration Layer]
        CustomElements[Custom Elements]
        EventSystem[Event System]
        Styling[Shared Styling]
    end
    
    ShellApp --> ComponentRegistry
    ComponentRegistry --> ShadowDOM
    ShadowDOM --> MFE1
    ShadowDOM --> MFE2
    ShadowDOM --> MFE3
    
    MFE1 --> CustomElements
    MFE2 --> CustomElements
    MFE3 --> CustomElements
    
    CustomElements --> Button
    CustomElements --> Card
    CustomElements --> Modal
    CustomElements --> Form
    
    style MainApp fill:#e8f5e8
    style MicroFrontends fill:#e3f2fd
    style Components fill:#fff3e0
    style Integration fill:#fce4ec
```

## Web Components Implementation

### Custom Component Definition

```javascript
// Custom Button Component
class CustomButton extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
  }

  connectedCallback() {
    this.render();
  }

  render() {
    const button = document.createElement('button');
    button.textContent = this.getAttribute('label') || 'Click me';
    button.style.cssText = `
      background-color: ${this.getAttribute('color') || '#007bff'};
      color: white;
      border: none;
      padding: 8px 16px;
      border-radius: 4px;
      cursor: pointer;
      font-family: inherit;
    `;

    button.addEventListener('click', () => {
      this.dispatchEvent(new CustomEvent('button-click', {
        detail: { value: this.getAttribute('value') }
      }));
    });

    this.shadowRoot.appendChild(button);
  }

  static get observedAttributes() {
    return ['label', 'color', 'value'];
  }

  attributeChangedCallback(name, oldValue, newValue) {
    if (name === 'label') {
      this.shadowRoot.querySelector('button').textContent = newValue;
    }
  }
}

// Register the custom element
customElements.define('custom-button', CustomButton);
```

### Micro-frontend Component Usage

```jsx
// Micro-frontend React Component
import React, { useEffect, useRef } from 'react';

const CustomButtonWrapper = ({ label, color, value, onClick }) => {
  const buttonRef = useRef(null);

  useEffect(() => {
    const button = buttonRef.current;
    if (button) {
      button.addEventListener('button-click', (event) => {
        onClick(event.detail.value);
      });
    }
  }, [onClick]);

  return (
    <custom-button
      ref={buttonRef}
      label={label}
      color={color}
      value={value}
    />
  );
};

// Usage in micro-frontend
function UserDashboard() {
  const handleButtonClick = (value) => {
    console.log('Button clicked with value:', value);
  };

  return (
    <div>
      <h2>User Dashboard</h2>
      <CustomButtonWrapper
        label="Save User"
        color="#28a745"
        value="save"
        onClick={handleButtonClick}
      />
      <CustomButtonWrapper
        label="Cancel"
        color="#dc3545"
        value="cancel"
        onClick={handleButtonClick}
      />
    </div>
  );
}
```

### Shell Application Integration

```javascript
// Shell Application - Component Loader
class ComponentLoader {
  constructor() {
    this.loadedComponents = new Map();
  }

  async loadComponent(componentName, version = 'latest') {
    if (this.loadedComponents.has(componentName)) {
      return this.loadedComponents.get(componentName);
    }

    try {
      const response = await fetch(`/components/${componentName}/${version}/component.js`);
      const componentCode = await response.text();
      
      // Create and register the component
      const script = document.createElement('script');
      script.textContent = componentCode;
      document.head.appendChild(script);
      
      this.loadedComponents.set(componentName, true);
      return true;
    } catch (error) {
      console.error(`Failed to load component ${componentName}:`, error);
      return false;
    }
  }

  async loadAllComponents(components) {
    const loadPromises = components.map(component => 
      this.loadComponent(component.name, component.version)
    );
    
    return Promise.allSettled(loadPromises);
  }
}

// Usage
const loader = new ComponentLoader();
await loader.loadAllComponents([
  { name: 'custom-button', version: '1.2.0' },
  { name: 'user-card', version: '2.0.0' },
  { name: 'data-table', version: '1.5.0' }
]);
```

---

[⬅️ Back to Micro-frontends](./README.md)
