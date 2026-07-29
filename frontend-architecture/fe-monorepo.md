# Frontend Monorepo

A monorepo is a single repository containing multiple distinct projects. For frontend teams, monorepos enable shared configuration, component libraries, type sharing across packages, and coordinated releases.

---

## Tooling

### Nx

Smart monorepo framework with dependency graph awareness, affected command detection, and task orchestration.

- **Key features**: Dependency graph visualization, computation caching, distributed task execution, code generation
- **Computation caching**: Replay build/test output when inputs haven't changed — drastically reduces CI times
- **Affected commands**: Run tasks only for projects affected by a given change: `nx affected --test`

### Turborepo

A lightweight monorepo orchestrator by Vercel, focused on caching and task orchestration.

- **Key features**: Remote caching (Vercel), parallel task execution, dependency-aware pipeline
- **Pipeline**: Declarative task dependencies and caching rules in `turbo.json`

### pnpm Workspaces

Package manager-level monorepo support with strict dependency isolation and disk-efficient storage.

- **Key features**: Content-addressable storage (saves disk space), strict dependency isolation (no phantom dependencies), `pnpm --filter` for targeted commands

---

## Monorepo Structure

A typical frontend monorepo layout:

```
packages/
  ui/            # Shared component library
  config-eslint/ # Shared ESLint configuration
  config-ts/     # Shared TypeScript configuration
apps/
  web/           # Main web app (Next.js)
  admin/         # Admin dashboard (Vite + React)
  docs/          # Documentation site (Astro)
```

---

## Comparison

| Tool | Caching | Task Orchestration | Remote Cache | Generator | Learning Curve |
|---|---|---|---|---|---|
| Nx | ✅ Advanced | ✅ Advanced | ✅ | ✅ | Steep |
| Turborepo | ✅ Basic | ✅ Pipeline | ✅ (Vercel) | ❌ | Low |
| pnpm | ❌ | ❌ | ❌ | ❌ | Low |
| Lerna | ❌ | Basic | ❌ | ❌ | Low |

---

[⬅️ Back to Frontend Architecture](./README.md)
