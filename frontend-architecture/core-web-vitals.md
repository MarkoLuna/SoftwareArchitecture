# Core Web Vitals & Performance

Core Web Vitals are a set of real-world metrics that measure user experience on the web. They are part of Google's ranking signals and a key indicator of site quality.

---

## Core Metrics

### LCP (Largest Contentful Paint)

Measures perceived load speed — the time until the largest visible element (image, video, or text block) is rendered.

- **Good**: ≤ 2.5s
- **Poor**: > 4.0s
- **Common causes**: Slow server response, render-blocking resources, large images
- **Improvements**: Preload key resources, optimize images (WebP/AVIF, responsive sizes), use CDN

### INP (Interaction to Next Paint)

Measures responsiveness — the time from a user interaction (click, tap, keypress) to the next visual update. Replaced FID in March 2024.

- **Good**: ≤ 200ms
- **Poor**: > 500ms
- **Common causes**: Long tasks on the main thread, heavy event handlers, layout thrashing
- **Improvements**: Break long tasks with `yield`, debounce input handlers, use Web Workers

### CLS (Cumulative Layout Shift)

Measures visual stability — the sum of unexpected layout shifts during the page's lifetime.

- **Good**: ≤ 0.1
- **Poor**: > 0.25
- **Common causes**: Images without dimensions, dynamically injected content (ads, embeds), web fonts causing layout shifts
- **Improvements**: Always set `width`/`height` on images, reserve space for ads, use `font-display: optional`

---

## Performance Patterns

### Lazy Loading

Defer loading of non-critical resources until they are needed.

- **Image lazy loading**: `<img loading="lazy">` (native, no JS)
- **Component lazy loading**: `React.lazy()` / dynamic imports for route-level splitting
- **Intersection Observer**: Load resources when elements enter the viewport

### Preloading and Prefetching

- **`<link rel="preload">`**: Load critical resources (fonts, hero images) immediately
- **`<link rel="prefetch">`**: Load resources for the next page during idle time
- **`<link rel="preconnect">`**: Warm up connections to third-party origins

### Bundle Optimization

- **Tree shaking**: Remove dead code via ES module static analysis
- **Code splitting**: Split application into route-level or component-level chunks
- **Compression**: Use Brotli (better than Gzip) on supported origins

### Caching

- **HTTP caching**: `Cache-Control` headers for static assets (immutable content, long max-age)
- **Service Worker cache**: Cache API for offline support and faster repeat visits
- **Memory cache**: In-memory caches for computed values (memoization, `useMemo`)

---

## Measurement Tools

| Tool | Type | Use Case |
|---|---|---|
| Lighthouse | Lab (simulated) | CI/CD audits, regression detection |
| PageSpeed Insights | Lab + Field | Public URL analysis with CrUX data |
| Web Vitals Extension | Field (real user) | Local debugging of real user metrics |
| Chrome DevTools Performance | Lab (profiled) | Deep diagnostic of specific issues |

---

[⬅️ Back to Frontend Architecture](./README.md)
