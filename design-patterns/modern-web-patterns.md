# 🌐 Modern/Web Patterns

Modern design patterns focused on frontend development, Node.js, and modern web architectures.

---

## 1. Module Pattern
Splits code into smaller, reusable, and isolated pieces. It helps in managing dependencies and avoiding global namespace pollution.

### JavaScript Implementation (ES Modules)
```javascript
// logger.js
const privatePrefix = "[LOG]";

export const log = (message) => {
    console.log(`${privatePrefix} ${message}`);
};

// main.js
import { log } from './logger.js';

log("Module pattern in action!");
```

## 2. Mixin Pattern
Adds functionality to objects or classes without using inheritance. It allows for flexible code reuse by "mixing in" behaviors from multiple sources.

### JavaScript Implementation
```javascript
const flyMixin = {
    fly() {
        console.log(`${this.name} is flying!`);
    }
};

class Bird {
    constructor(name) {
        this.name = name;
    }
}

// Adding fly functionality to Bird without inheritance
Object.assign(Bird.prototype, flyMixin);

const parrot = new Bird("Parrot");
parrot.fly(); // Parrot is flying!
```

## 3. Provider Pattern
Provides data or state to multiple components in a tree structure without manual prop-passing (prop-drilling). Common in React (Context API) and other modern frontend frameworks.

### JavaScript Implementation (React-style)
```javascript
import React, { createContext, useContext } from 'react';

const ThemeContext = createContext();

// Provider Component
export const ThemeProvider = ({ children }) => {
    const theme = "dark";
    return (
        <ThemeContext.Provider value={theme}>
            {children}
        </ThemeContext.Provider>
    );
};

// Consumer Component
export const ThemeButton = () => {
    const theme = useContext(ThemeContext);
    return <button className={theme}>I am {theme}!</button>;
};
```

---

[⬅️ Back to Design Patterns](./README.md)
