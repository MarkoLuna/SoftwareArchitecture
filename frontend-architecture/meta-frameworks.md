# Meta-Frameworks

Meta-frameworks are full-stack frameworks built on top of UI libraries (React, Vue, Svelte) that provide routing, data fetching, build configuration, and deployment strategies out of the box.

---

## Next.js (React)

The most popular React meta-framework, developed by Vercel.

- **Rendering**: SSR, SSG, ISR, and (since v13) React Server Components (RSC)
- **Routing**: File-based (`app/` directory with `page.tsx`, `layout.tsx`, `loading.tsx`, `error.tsx`)
- **Data fetching**: Server-side `async` components, `fetch()` with automatic memoization and caching
- **Key concepts**:
  - **App Router**: Nested layouts, loading states, error boundaries via file system
  - **Server Components**: Components that render on the server and send only HTML — zero JS for static content
  - **Server Actions**: Mutations handled by server-side functions called directly from client components
- **Deployment**: Vercel (optimized), Node.js servers, static export

---

## Nuxt (Vue)

The full-stack framework for Vue, developed by the Nuxt Labs team.

- **Rendering**: SSR, SSG, ISR, and hybrid per-route configuration
- **Routing**: File-based (`pages/` directory with Vue components)
- **Key concepts**:
  - **Auto-imports**: Composables, components, and utility functions auto-imported from `composables/`, `components/`
  - **Modules**: Plugin ecosystem (Auth, Content, Tailwind, i18n)
  - **Nitro Server**: Built-in server engine for API routes and middleware
- **Deployment**: Node.js, serverless (Netlify, Vercel, Cloudflare), static export

---

## Remix (React)

A React meta-framework focused on web standards and progressive enhancement, developed by the Shopify team.

- **Rendering**: SSR-only with progressive enhancement; no SSG/ISR
- **Routing**: File-based (`routes/` directory) with nested routes
- **Data fetching**: `loader` functions (runs on server, feeds data to route components), `action` functions (handles form submissions)
- **Key concepts**:
  - **Nested routes**: Each segment of the URL has its own data requirements — parent and child routes fetch independently
  - **Web standards**: Uses Fetch API, `Request`/`Response`, web streams natively
  - **Progressive enhancement**: Forms work without JavaScript; JS enhances the experience
- **Deployment**: Any Node.js or serverless runtime (Fly.io, Cloudflare, Netlify, Vercel)

---

## SvelteKit (Svelte)

The official framework for Svelte, built on top of the Vite ecosystem.

- **Rendering**: SSR, SSG, and hybrid per-route
- **Routing**: File-based (`routes/` directory) with optional `+page.svelte`, `+layout.svelte`, `+server.js` convention
- **Data fetching**: `load` functions run on server or client depending on configuration
- **Key concepts**:
  - **Form actions**: Built-in form handling with progressive enhancement
  - **Adaptors**: Platform-specific deployment targets (`@sveltejs/adapter-node`, `adapter-vercel`, `adapter-cloudflare`, `adapter-static`)
  - **Zero JS by default**: Components with no interactivity ship zero client-side JavaScript
- **Deployment**: Node.js, serverless, static, or edge (Cloudflare Workers)

---

## Comparison

| Aspect | Next.js | Nuxt | Remix | SvelteKit |
|---|---|---|---|---|
| UI Library | React | Vue 3 | React | Svelte |
| Rendering | SSR, SSG, ISR, RSC | SSR, SSG, ISR, hybrid | SSR only | SSR, SSG, hybrid |
| Routing | File-based (App Router) | File-based | File-based (nested) | File-based |
| Data Fetching | Server Components / `fetch` | `useAsyncData` / `useFetch` | `loader` / `action` | `load` functions |
| Server Functions | Server Actions | `server/api/` routes | `action` functions | Form actions + server routes |
| Deployment | Vercel, Node, static | Node, serverless, static | Node, serverless | Node, serverless, edge, static |
| Bundle Size | Medium (RSC reduces JS) | Medium | Small (no runtime) | Smallest (compiled) |

---

[⬅️ Back to Frontend Architecture](./README.md)
