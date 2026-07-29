# API Client Architecture

The API client layer manages communication between the frontend and backend. Its design affects caching, error handling, optimistic updates, and developer experience.

---

## Approaches

### Fetch API

The native browser API for making HTTP requests. Minimalist and zero-dependency.

- **Pros**: No dependencies, standard across browsers, native streaming
- **Cons**: No request/response interceptors, no automatic caching, no query deduplication
- **Best for**: Simple projects or when you need minimal abstraction

### Axios

A promise-based HTTP client with a richer API than fetch.

- **Pros**: Request/response interceptors, automatic JSON parsing, request cancellation (`AbortController`), progress events, wider browser support
- **Cons**: Adds ~14KB to the bundle, not tree-shakeable
- **Best for**: Projects needing interceptors, cancellation, or progress monitoring

### tRPC

TypeScript-native RPC framework that provides end-to-end type safety without code generation.

- **Pros**: Full type safety from server to client, no schema duplication, auto-complete for API endpoints, minimal boilerplate
- **Cons**: Server must use TypeScript, no RESTful URL structure, less tooling for non-TypeScript consumers
- **Best for**: Full-stack TypeScript monorepos

### RTK Query

Data fetching and caching layer built into Redux Toolkit.

- **Pros**: Automatic caching and cache invalidation, built-in optimistic updates, integration with Redux DevTools, tag-based invalidation
- **Cons**: Requires Redux (heavy if not already using it), learning curve for the query/mutation model
- **Best for**: Applications already using Redux

### Apollo Client (GraphQL)

A comprehensive GraphQL client for managing queries, mutations, subscriptions, and local state.

- **Pros**: Declarative data fetching, normalized cache, subscriptions support, optimistic UI, pagination helpers
- **Cons**: Overhead for simple REST APIs, larger bundle, complex cache normalization logic
- **Best for**: Applications consuming GraphQL APIs

---

## Architecture Patterns

### Repository Pattern

Abstract the data layer behind a repository interface. Components call repositories, not HTTP clients directly.

```
Component → Repository → HTTP Client → API

Repository: getUsers(), createUser(data), deleteUser(id)
```

### Service Layer

Thin service modules that encapsulate API calls, transformations, and error handling. Keeps components clean.

```js
// userService.js
export const userService = {
  async getAll() { return api.get('/users'); },
  async getById(id) { return api.get(`/users/${id}`); },
  async create(data) { return api.post('/users', data); },
};
```

### TanStack Query (React Query)

Server state management library that handles caching, background refetching, pagination, and optimistic updates.

- **Key concepts**: `useQuery` (reads), `useMutation` (writes), `queryClient` (cache manager), stale time, cache time
- **DevTools**: Built-in React devtools for inspecting cache state

---

## Comparison

| Client | Type | Bundle Size | Caching | Type Safety | Best For |
|---|---|---|---|---|---|
| Fetch | REST | 0 KB | Manual | Manual | Simple projects |
| Axios | REST | ~14 KB | Manual | Manual | Interceptors, cancellation |
| tRPC | RPC | ~5 KB | Manual | End-to-end | Full-stack TS |
| RTK Query | REST/GraphQL | ~12 KB | Auto | Manual | Redux apps |
| Apollo | GraphQL | ~40 KB | Auto | Via codegen | GraphQL APIs |
| TanStack Query | Any | ~12 KB | Auto | Manual | Server state management |

---

[⬅️ Back to Frontend Architecture](./README.md)
