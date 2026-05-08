# ⚡ Caching Strategies

Caching is a powerful technique to improve the performance and scalability of a system by storing frequently accessed data in a fast-access storage layer.

---

## 🗺️ Table of Contents
1. [Caching Patterns](#1-caching-patterns)
2. [Eviction Policies](#2-eviction-policies)
3. [Caching Levels](#3-caching-levels)
4. [Cache Invalidation](#4-cache-invalidation)

---

## 1. Caching Patterns

### Cache-Aside (Lazy Loading)
The application first checks the cache. If the data is not there (cache miss), it fetches it from the database and stores it in the cache for future use.
- **Pros**: Only data that is actually requested is cached.
- **Cons**: First request is always slow.

### Read-Through
The application treats the cache as the main data store. If a cache miss occurs, the cache itself is responsible for fetching data from the database and updating itself before returning it to the application.

### Write-Through
Every write operation goes through the cache and is immediately written to the database.
- **Pros**: Cache is always up-to-date with the database.
- **Cons**: Write latency is increased.

### Write-Behind (Write-Back)
The application writes data to the cache, and the cache asynchronously writes the data to the database after some delay.
- **Pros**: Extremely fast write performance.
- **Cons**: Risk of data loss if the cache fails before writing to the database.

---

## 2. Eviction Policies
When the cache is full, it must decide which data to remove to make room for new entries.

- **LRU (Least Recently Used)**: Discards the least recently used items first.
- **LFU (Least Frequently Used)**: Discards the items that are used least often.
- **FIFO (First In, First Out)**: Discards the oldest items first.

---

## 3. Caching Levels

| Level | Description |
| :--- | :--- |
| **Client-side** | Browser cache, local storage. |
| **CDN** | Geographically distributed servers that cache static content close to users. |
| **Web Server** | Nginx/Apache caching for static assets or full-page responses. |
| **Application** | In-memory caches (Redis, Memcached) for session data or DB query results. |
| **Database** | Database-level buffer pools and query caches. |

---

## 4. Cache Invalidation
"There are only two hard things in Computer Science: cache invalidation and naming things." — *Phil Karlton*

- **TTL (Time to Live)**: Data automatically expires after a certain duration.
- **Event-based Invalidation**: Use domain events (e.g., `OrderUpdated`) to explicitly remove or update cache entries.
- **Versioning**: Include a version number in the cache key.
