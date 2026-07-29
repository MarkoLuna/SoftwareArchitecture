# Bundlers & Build

Frontend bundlers are tools that take application source code (JavaScript, TypeScript, CSS, assets) and produce optimized bundles for the browser. The choice of bundler affects development speed, build time, tree-shaking effectiveness, code splitting, and caching strategy.

---

## Webpack

The most mature and configurable bundler. Uses a dependency graph based on import/require statements. Ecosystem includes loaders (transform files before bundling) and plugins (apply broader build optimizations).

- **Config file**: `webpack.config.js`
- **Key concept**: Loaders transform files (Babel, CSS, TypeScript), plugins optimize output (Minimizer, HTML generation)
- **Code splitting**: `import()` dynamic imports, `SplitChunksPlugin`, `EntryPoint` splitting
- **Module Federation**: Built-in plugin for micro-frontends

---

## Vite

A next-generation bundler that uses **esbuild** for development (instant Hot Module Replacement) and **Rollup** for production builds. Zero-config by default with sensible presets for frameworks.

- **Dev server**: Native ESM, no bundling during development — serves files directly to the browser
- **Production**: Rollup-based with tree-shaking and code splitting
- **Framework support**: First-class plugins for React, Vue, Svelte, Lit, Solid

### Vite vs Webpack

| Aspect | Vite | Webpack |
|---|---|---|
| Dev server cold start | Instant (esbuild pre-bundle) | Slow (full bundle rebuild) |
| HMR | Instant for any file size | Degrades with project size |
| Configuration | Minimal, convention-based | Verbose, loaders + plugins |
| Production build | Rollup | Webpack own |
| Plugin ecosystem | Growing (Rollup-compatible) | Mature and extensive |

---

## esbuild

An extremely fast bundler written in Go. Used internally by Vite for pre-bundling dependencies and by many tools for transformation tasks.

- **Speed**: 10-100x faster than Node.js-based bundlers
- **Limitation**: No built-in Hot Module Replacement, limited plugin API
- **Primary use**: Dependency pre-bundling, TypeScript/JSX transformation, minification

---

## Turbopack

A Rust-based incremental bundler by the Vercel team (creators of Next.js). Designed as a successor to Webpack for large-scale applications.

- **Incremental computation**: Only rebuilds what changed
- **Speed**: Claims 10x faster than Vite and 700x faster than Webpack on large apps
- **Current status**: In alpha; used as the development bundler in Next.js 14+

---

## Key Build Concepts

### Tree Shaking

Dead-code elimination that removes unused exports during the bundling process. Requires ES module syntax (`import`/`export`). Works most effectively with side-effect-free modules.

### Code Splitting

Splitting the application into smaller chunks loaded on demand. Strategies:

- **Entry point splitting**: Separate bundles per entry page
- **Dynamic imports**: `import()` expressions create automatic split points
- **Vendor splitting**: Separate `vendor` chunk for third-party dependencies (better caching)

### Bundle Analysis

Tools to visualize what's in your bundles and identify optimization opportunities:

- `webpack-bundle-analyzer`
- `vite-bundle-visualizer`
- `source-map-explorer`

---

[⬅️ Back to Frontend Architecture](./README.md)
