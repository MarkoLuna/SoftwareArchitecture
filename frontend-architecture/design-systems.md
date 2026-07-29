# Design Systems & Component Composition

A design system is a collection of reusable components, guided by clear standards, that can be assembled to build any number of applications. It combines design tokens, component libraries, documentation, and tooling.

---

## Component Composition Patterns

### Compound Components

A parent component manages implicit state shared by child components. Children must be used inside the parent.

```jsx
<Select>
  <Select.Trigger>Open</Select.Trigger>
  <Select.Options>
    <Select.Option value="1">Option 1</Select.Option>
    <Select.Option value="2">Option 2</Select.Option>
  </Select.Options>
</Select>
```

### Polymorphic Components

A component that renders as a different HTML element based on a prop, while preserving its semantics and styling.

```jsx
<Button as="a" href="/page">Link Button</Button>
<Button as="button" onClick={handleClick}>Action Button</Button>
```

### Controlled vs Uncontrolled

- **Controlled**: Parent manages state via props (`value`, `onChange`)
- **Uncontrolled**: Component manages its own internal state via refs
- **Hybrid**: Support both modes with a `defaultValue` fallback

---

## Provider Pattern

The Provider Pattern uses React Context (or similar) to pass data through the component tree without prop drilling. It is commonly used for theming, authentication, and global state.

```jsx
const ThemeContext = React.createContext();

const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light');
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

const ThemedButton = () => {
  const { theme } = useContext(ThemeContext);
  return <button className={theme}>Themed Button</button>;
};
```

### When to Use Provider

- Global theme or localization
- Authentication/user context
- Feature flags
- Shared state across distant component subtrees

### Performance Considerations

- **Value stability**: Memoize the context value to prevent unnecessary re-renders
- **Splitting contexts**: Separate fast-changing and stable values into different providers
- **Selective subscriptions**: Libraries like Zustand or Jotai solve this at the state level

---

## Storybook

Storybook is the industry standard for developing, documenting, and testing UI components in isolation.

- **Component playground**: Develop components without running the full app
- **Documentation**: Auto-generated docs with props tables, descriptions, and usage examples
- **Testing**: Integration with Playwright, Vitest, and accessibility audit tools
- **Addons**: Controls, Actions, Viewport, Accessibility, Theming

---

[⬅️ Back to Frontend Architecture](./README.md)
