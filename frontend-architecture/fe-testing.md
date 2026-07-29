# Frontend Testing

Frontend testing spans multiple layers from unit tests for individual functions to end-to-end tests that simulate real user flows.

---

## Testing Layers

### Unit Tests

Test individual functions, hooks, utility modules, or state logic in isolation. Fast and reliable.

- **Vitest**: Fast Vite-native test runner, compatible with Jest API
- **Jest**: Mature test runner with snapshot testing, coverage, and mocking
- **React Testing Library**: Tests components by user behavior (queries by accessible roles, text, labels)

### Component Tests

Test a component's rendering, interaction, and state changes.

```jsx
describe('Button', () => {
  it('calls onClick when clicked', async () => {
    const onClick = vi.fn();
    render(<Button onClick={onClick}>Click</Button>);
    await userEvent.click(screen.getByRole('button'));
    expect(onClick).toHaveBeenCalledOnce();
  });
});
```

### Integration Tests

Test how components work together — form submission flows, list filtering, navigation state.

### End-to-End (E2E) Tests

Test critical user flows in a real browser against a running application.

- **Playwright**: Cross-browser automation with auto-wait, trace viewer, and parallel execution
- **Cypress**: All-in-one E2E framework with time-travel debugging and network stubbing
- **Key flows to cover**: Authentication, search, checkout, error scenarios

---

## Visual Regression Testing

Captures screenshots of components or pages and compares them against baselines to detect unintended visual changes.

- **Playwright Visual Comparisons**: Built-in `toHaveScreenshot()`
- **Chromatic**: Hosted visual testing for Storybook components
- **Percy**: Cross-browser visual diffing in CI

---

## Accessibility Testing

Automated checks for WCAG compliance integrated into the test pipeline.

- **axe-core** (Deque): Industry-standard engine, integrates with Cypress, Playwright, and unit tests
- **Lighthouse CI**: Enforce accessibility audits as part of CI

---

## Framework Comparison

| Tool | Type | Speed | Best For |
|---|---|---|---|
| Vitest | Unit/Integration | Very fast | Vite-based projects |
| Playwright | E2E | Fast (parallel) | Cross-browser E2E |
| Cypress | E2E | Moderate | Interactive debugging |
| Storybook + Chromatic | Visual | Moderate | Design system visual testing |

---

[⬅️ Back to Frontend Architecture](./README.md)
