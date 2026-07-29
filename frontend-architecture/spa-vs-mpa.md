# SPA vs MPA

Single Page Applications (SPA) and Multi-Page Applications (MPA) represent two fundamental approaches to web application architecture.

---

## SPA (Single Page Application)

The entire application loads as a single HTML page. Navigation happens client-side — JavaScript replaces the view content without full page reloads.

- **Rendering**: Client-side rendering (CSR) or server-side rendering (SSR) via meta-frameworks
- **Navigation**: Client-side routing (`react-router`, `vue-router`)
- **State**: Persisted in-memory across navigation
- **Frameworks**: React, Vue, Svelte, Angular

### Pros
- Snappy navigation after initial load (no full page reloads)
- Rich, app-like interactions
- Persisted state across views (no refetch)
- Works offline with service workers

### Cons
- Slower initial load (large JS bundle)
- Heavier client device resource usage
- SEO requires SSR/SSG workarounds
- JavaScript-dependent (progressive enhancement harder)

---

## MPA (Multi-Page Application)

Each navigation triggers a full page reload from the server. The server sends a complete HTML document for each request.

- **Rendering**: Server-side rendering (traditional)
- **Navigation**: Full-page reload with HTTP requests
- **State**: Typically stored on the server (sessions) or in cookies
- **Frameworks**: Traditional server frameworks (Rails, Django, Laravel, PHP)

### Pros
- Fast initial page load (minimal JavaScript)
- Natural SEO (server renders full HTML)
- Graceful degradation without JavaScript
- Lower client resource usage
- Simpler mental model — each page is independent

### Cons
- Slower navigation (full page reload on every click)
- State lost between pages (re-fetch on each request)
- Harder to build rich, app-like interactions
- More server load per page transition

---

## Decision Matrix

| Factor | SPA | MPA |
|---|---|---|
| Initial load time | Slower | Faster |
| Navigation speed | Instant | Reload |
| SEO (no SSR) | Poor | Excellent |
| Offline support | Good | Limited |
| Device resources | Higher | Lower |
| Development complexity | Higher | Lower |
| Best for | Dashboards, tools, SaaS | Content sites, e-commerce, blogs |
| Framework examples | React, Vue, Svelte | Rails, Django, Laravel |

---

## Hybrid Approach

Modern meta-frameworks blur the line:

- **Next.js/Nuxt/SvelteKit**: Serve MPA-style HTML on initial load, then hydrate into a SPA for subsequent navigations
- **Islands architecture**: Interactive widgets (SPA-like components) embedded in static HTML (Marko, Astro)
- **Transitional apps**: Start as MPA, progressively add SPA capabilities where needed

---

[⬅️ Back to Frontend Architecture](./README.md)
