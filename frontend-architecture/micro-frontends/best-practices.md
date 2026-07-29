# Micro-frontends — Best Practices

## Strategy Comparison

### Decision Matrix

| Strategy | Independence | Performance | Complexity | Best For |
|----------|-------------|------------|-----------|----------|
| **Module Federation** | High | Excellent | Medium | Large teams, different frameworks |
| **Iframe** | Medium | Good | Low | Legacy integration, security boundaries |
| **Web Components** | High | Good | High | Component reuse, design systems |
| **Hybrid** | Medium | Good | High | Complex requirements |

### Use Case Guidelines

#### Choose Module Federation When:
- Multiple teams with different tech stacks
- Need for runtime dependency sharing
- Complex inter-component communication
- Large-scale applications
- Requirement for independent deployment

#### Choose Iframe When:
- Legacy application integration
- Strong security boundaries required
- Simple communication needs
- Third-party integration
- Minimal technical overhead desired

#### Choose Web Components When:
- Design system consistency critical
- Component reuse across applications
- Framework-agnostic components needed
- Progressive enhancement strategy
- Multiple target platforms

---

## Implementation Patterns

### Shared State Management

```javascript
// Cross-micro-frontend state management
class MicroFrontendState {
  constructor() {
    this.state = new Map();
    this.subscribers = new Set();
  }

  setState(key, value) {
    this.state.set(key, value);
    this.notifySubscribers(key, value);
  }

  getState(key) {
    return this.state.get(key);
  }

  subscribe(key, callback) {
    const subscriber = { key, callback };
    this.subscribers.add(subscriber);
    
    // Return unsubscribe function
    return () => {
      this.subscribers.delete(subscriber);
    };
  }

  notifySubscribers(key, value) {
    this.subscribers.forEach(subscriber => {
      if (subscriber.key === key) {
        subscriber.callback(value);
      }
    });
  }
}

// Usage in shell application
const globalState = new MicroFrontendState();

// Subscribe to user state changes
const unsubscribeUser = globalState.subscribe('user', (userData) => {
  updateNavigation(userData);
  updateHeader(userData);
});

// Update user state from any micro-frontend
globalState.setState('user', { name: 'John', email: 'john@example.com' });
```

### Routing and Navigation

```javascript
// Cross-micro-frontend routing
class MicroFrontendRouter {
  constructor() {
    this.routes = new Map();
    this.currentRoute = null;
  }

  registerRoute(pattern, microFrontend) {
    this.routes.set(pattern, microFrontend);
  }

  navigate(path, params = {}) {
    const matchedRoute = this.findMatchingRoute(path);
    
    if (matchedRoute) {
      this.loadMicroFrontend(matchedRoute.microFrontend, {
        path,
        params
      });
      this.currentRoute = matchedRoute;
    }
  }

  findMatchingRoute(path) {
    for (const [pattern, microFrontend] of this.routes) {
      if (this.matchesPattern(path, pattern)) {
        return { pattern, microFrontend };
      }
    }
    return null;
  }

  matchesPattern(path, pattern) {
    // Simple pattern matching (can be enhanced with regex)
    return path.startsWith(pattern.replace('*', ''));
  }

  async loadMicroFrontend(microFrontend, context) {
    // Load the appropriate micro-frontend
    if (microFrontend === 'module-federation') {
      await this.loadModuleFederationApp(microFrontend, context);
    } else if (microFrontend === 'iframe') {
      await this.loadIframeApp(microFrontend, context);
    } else if (microFrontend === 'web-components') {
      await this.loadWebComponentsApp(microFrontend, context);
    }
  }
}
```

### Error Boundaries and Fallbacks

```javascript
// Error boundary for micro-frontends
class MicroFrontendErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, errorInfo) {
    this.setState({ error, errorInfo });
    
    // Log error to monitoring service
    this.logError(error, errorInfo);
    
    // Show fallback UI
    this.showFallbackUI();
  }

  logError(error, errorInfo) {
    console.error('Micro-frontend error:', error, errorInfo);
    
    // Send to error tracking service
    fetch('/api/error-tracking', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        error: error.message,
        stack: error.stack,
        component: this.props.componentName,
        timestamp: new Date().toISOString()
      })
    });
  }

  showFallbackUI() {
    // Show user-friendly error message
    this.setState({
      hasError: true,
      error: {
        message: 'Something went wrong. Please try again.',
        action: 'retry'
      }
    });
  }

  render() {
    if (this.state.hasError) {
      return (
        <div className="error-fallback">
          <h2>Oops! Something went wrong</h2>
          <p>{this.state.error.message}</p>
          <button onClick={() => window.location.reload()}>
            {this.state.error.action}
          </button>
        </div>
      );
    }

    return this.props.children;
  }
}

// Usage
const SafeMicroFrontend = () => (
  <MicroFrontendErrorBoundary componentName="UserDashboard">
    <UserDashboard />
  </MicroFrontendErrorBoundary>
);
```

---

## Best Practices

### Performance Optimization

```javascript
// Lazy loading and code splitting
const LazyMicroFrontend = React.lazy(() => 
  import('./micro-frontend').then(module => module.MicroFrontend)
);

// Preload critical components
const preloadComponent = (componentName) => {
  const link = document.createElement('link');
  link.rel = 'prefetch';
  link.href = `/components/${componentName}/component.js`;
  document.head.appendChild(link);
};

// Resource optimization
const optimizeBundle = {
  optimization: {
    splitChunks: 'all',
    chunkIds: 'named',
    runtimeChunk: 'single',
  },
  performance: {
    hints: false,
    maxEntrypointSize: 244000,
    maxAssetSize: 244000,
  }
};
```

### Security Considerations

```javascript
// Content Security Policy
const CSP_DIRECTIVES = {
  'default-src': "'self'",
  'script-src': "'self' 'unsafe-inline'",
  'style-src': "'self' 'unsafe-inline'",
  'img-src': "'self' data: https:",
  'connect-src': "'self' https://api.example.com",
  'frame-ancestors': "'self' https://shell.example.com",
};

// Set CSP headers
app.use((req, res, next) => {
  res.setHeader('Content-Security-Policy', Object.entries(CSP_DIRECTIVES)
    .map(([key, value]) => `${key} ${value}`)
    .join('; ')
  );
  next();
});

// Message origin validation
const validateMessageOrigin = (event, allowedOrigins) => {
  return allowedOrigins.includes(event.origin);
};
```

### Testing Strategies

```javascript
// Integration testing for micro-frontends
describe('Micro-frontend Integration', () => {
  let shellApp, microFrontend;

  beforeEach(() => {
    // Set up test environment
    shellApp = createTestShellApp();
    microFrontend = createTestMicroFrontend();
  });

  afterEach(() => {
    // Clean up test environment
    shellApp.destroy();
    microFrontend.destroy();
  });

  test('should load micro-frontend in shell', async () => {
    await shellApp.loadMicroFrontend('user-dashboard');
    
    expect(shellApp.getLoadedMicroFrontend('user-dashboard')).toBeTruthy();
    expect(shellApp.isMicroFrontendVisible('user-dashboard')).toBeTruthy();
  });

  test('should handle communication between shell and micro-frontend', async () => {
    const messagePromise = new Promise(resolve => {
      microFrontend.onMessage(resolve);
    });

    await shellApp.loadMicroFrontend('user-dashboard');
    shellApp.sendMessageToMicroFrontend('user-dashboard', { type: 'PING' });

    const message = await messagePromise;
    expect(message.type).toBe('PONG');
  });

  test('should handle micro-frontend errors gracefully', async () => {
    microFrontend.simulateError();
    
    await shellApp.loadMicroFrontend('user-dashboard');
    
    expect(shellApp.getErrorBoundary('user-dashboard')).toBeTruthy();
    expect(shellApp.getFallbackUI('user-dashboard')).toBeDefined();
  });
});
```

### Monitoring and Observability

```javascript
// Performance monitoring
class MicroFrontendMonitor {
  constructor() {
    this.metrics = new Map();
  }

  trackLoadTime(microFrontend, loadTime) {
    this.metrics.set(`${microFrontend}-load-time`, loadTime);
    this.sendMetrics({
      metric: 'load-time',
      microFrontend,
      value: loadTime,
      timestamp: Date.now()
    });
  }

  trackError(microFrontend, error) {
    this.metrics.set(`${microFrontend}-error-count`, 
      (this.metrics.get(`${microFrontend}-error-count`) || 0) + 1
    );
    
    this.sendMetrics({
      metric: 'error',
      microFrontend,
      error: error.message,
      stack: error.stack,
      timestamp: Date.now()
    });
  }

  trackUserInteraction(microFrontend, action) {
    this.sendMetrics({
      metric: 'user-interaction',
      microFrontend,
      action,
      timestamp: Date.now()
    });
  }

  sendMetrics(data) {
    // Send to monitoring service
    fetch('/api/metrics', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });
  }
}
```

---

## Getting Started

### Implementation Roadmap

1. **Assessment**: Analyze application requirements and team structure
2. **Strategy Selection**: Choose appropriate integration approach
3. **Architecture Design**: Define boundaries and communication patterns
4. **Implementation**: Start with pilot micro-frontend
5. **Testing**: Comprehensive testing strategy
6. **Deployment**: Gradual rollout with monitoring
7. **Optimization**: Performance and security improvements

### Project Structure Template

```
micro-frontend-app/
├── shell-app/
│   ├── src/
│   │   ├── components/
│   │   ├── routing/
│   │   └── state/
│   ├── public/
│   └── package.json
├── micro-frontends/
│   ├── user-dashboard/
│   │   ├── src/
│   │   ├── webpack.config.js
│   │   └── package.json
│   ├── product-catalog/
│   └── admin-panel/
├── shared-components/
│   ├── src/
│   │   ├── button/
│   │   ├── card/
│   │   └── form/
│   └── package.json
├── build-tools/
│   ├── webpack.common.js
│   └── deployment/
├── tests/
│   ├── integration/
│   └── e2e/
└── docs/
    ├── architecture.md
    └── deployment.md
```

---

## Further Reading

- [Module Federation Documentation](https://webpack.js.org/concepts/module-federation/)
- [Micro-frontends Best Practices](https://martinfowler.com/articles/micro-frontends.html)
- [Web Components Standards](https://www.w3.org/TR/webcomponents/)
- [Iframe Security Guide](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe)
- [Progressive Enhancement](https://www.smashingmagazine.com/2016/03/introduction-progressive-enhancement/)

---

[⬅️ Back to Micro-frontends](./README.md)
