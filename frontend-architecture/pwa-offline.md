# PWA & Offline

Progressive Web Apps (PWA) are web applications that use modern browser capabilities to deliver app-like experiences — installable, reliable, and capable of working offline.

---

## Service Workers

A service worker is a JavaScript file that runs in the background, separate from the web page, acting as a programmable network proxy.

- **Lifecycle**: `install` → `activate` → `fetch` interception
- **Scope**: Controls a path and its subpaths
- **Capabilities**:
  - Intercept and respond to network requests
  - Cache resources programmatically
  - Push notifications
  - Background sync

### Basic Service Worker Registration

```js
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js');
}
```

---

## Caching Strategies

### Cache First

Serve from cache first; fall back to network. Use for static assets (images, fonts, CSS).

```
fetch → check cache → found? → serve from cache
                      → not found? → fetch from network → cache → respond
```

### Network First

Try network first; fall back to cache. Use for API responses or content that changes frequently.

```
fetch → network request → success? → cache → respond
                        → failure? → serve from cache
```

### Stale-while-revalidate

Serve from cache immediately, then fetch from network in the background to update the cache for next time. Good for non-critical data.

```
fetch → serve cache immediately → fetch network in background → update cache
```

### Network Only

Do not involve the cache. Use for sensitive operations (payments, authentication).

---

## Offline Data Storage

| API | Type | Capacity | Persistence |
|---|---|---|---|
| Cache API | HTTP request/response pairs | Limited by disk | Until evicted |
| IndexedDB | Structured objects (NoSQL) | Large | Until evicted |
| localStorage | Key-value strings | 5-10 MB | Synchronous |

---

## Manifest File

The `manifest.json` file tells the browser about the PWA and enables the "Add to Home Screen" prompt.

```json
{
  "name": "My PWA",
  "short_name": "PWA",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#3b82f6",
  "icons": [
    { "src": "/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/icon-512.png", "sizes": "512x512", "type": "image/png" }
  ]
}
```

---

[⬅️ Back to Frontend Architecture](./README.md)
