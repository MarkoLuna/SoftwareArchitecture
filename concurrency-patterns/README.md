# 🔒 Concurrency & Thread Locks

Concurrency is the ability of different parts or units of a program, algorithm, or problem to be executed out-of-order or in partial order, without affecting the final outcome. Thread locks are synchronization primitives used to manage access to shared resources.

---

## 🗺️ Table of Contents
1. [Mutex (Mutual Exclusion)](#1-mutex-mutual-exclusion)
2. [Read/Write Locks](#2-readwrite-locks)
3. [Optimistic vs Pessimistic Locking](#3-optimistic-vs-pessimistic-locking)
4. [Deadlocks](#4-deadlocks)
5. [Semaphores](#5-semaphores)
6. [Active Object](#6-active-object)
7. [Producer-Consumer](#7-producer-consumer)
8. [Thread Pool](#8-thread-pool)
9. [Future and Promise](#9-future-and-promise)

---

## 1. Mutex (Mutual Exclusion)
A Mutex is a locking mechanism used to synchronize access to a resource. Only one thread can acquire the mutex at a time.
- **Workflow**: Lock -> Access Resource -> Unlock.
- **Danger**: Forgetting to unlock (use `defer` in Go or `try-finally` in Java).

---

## 2. Read/Write Locks
A more granular lock that allows multiple threads to read a resource simultaneously, but requires exclusive access for writing.
- **Readers**: Multiple concurrent readers allowed if no writer is active.
- **Writers**: Only one writer allowed, and no readers allowed while writing.
- **Best for**: Resources that are read frequently but updated rarely.

---

## 3. Optimistic vs Pessimistic Locking

### Pessimistic Locking
Assumes that conflicts will happen. A lock is acquired before the operation starts and released after it finishes.
- **Use case**: High contention, high risk of conflicts.

### Optimistic Locking
Assumes that conflicts are rare. The operation proceeds without a lock, but before committing, it checks if another thread has modified the data (usually via a version number).
- **Use case**: Low contention, web applications, distributed systems.

---

## 4. Deadlocks
A situation where two or more threads are blocked forever, waiting for each other to release locks.

### Necessary Conditions (Coffman Conditions)
1. **Mutual Exclusion**: At least one resource is held in a non-shareable mode.
2. **Hold and Wait**: A thread is holding at least one resource and waiting for another.
3. **No Preemption**: Resources cannot be taken away from a thread.
4. **Circular Wait**: A closed chain of threads exists where each is waiting for a resource held by the next.

### Avoidance Strategies
- **Lock Ordering**: Always acquire locks in the same predefined order.
- **Lock Timeout**: Don't wait indefinitely; release held locks if a new one can't be acquired.

---

## 5. Semaphores
A semaphore maintains a set of permits. Threads "acquire" a permit to access a resource and "release" it when done.
- **Binary Semaphore**: Equivalent to a Mutex (0 or 1).
- **Counting Semaphore**: Allows a fixed number of concurrent accesses (e.g., limiting database connections).

### Distributed Semaphores
In a distributed system, a traditional in-memory semaphore doesn't work across multiple server instances. A distributed semaphore uses an external coordinator:

- **Redis-based**: Uses Redis keys and atomic increments/decrements. Often implemented with Lua scripts to ensure atomicity.
- **Zookeeper/Consul-based**: Uses ephemeral nodes. If a client holding a permit crashes, the ephemeral node is deleted, and the permit is automatically released.
- **Leases**: Distributed semaphores must have a timeout (lease) to prevent a permit from being held forever if a consumer fails to release it.

---

## ⚖️ Locking Comparison

| Type | Concurrency Level | Complexity | Best Use Case |
| :--- | :--- | :--- | :--- |
| **Mutex** | Low | Low | Protecting a simple variable. |
| **Read/Write** | Medium | Medium | Shared caches, configurations. |
| **Optimistic** | ✅ High | Medium | Databases, distributed systems. |
| **Semaphore** | Medium | High | Resource pooling, rate limiting. |

---

## 6. Active Object
Decouples method execution from method invocation. It enhances concurrency and simplifies synchronized access to objects that reside in their own threads of control.
- **Mechanism**: A proxy receives method calls and converts them into "method requests" which are stored in a queue. A scheduler then picks requests from the queue and executes them in the active object's thread.

---

## 7. Producer-Consumer
One or more threads (producers) create data and add it to a shared buffer, while one or more other threads (consumers) take the data from the buffer and process it.
- **Synchronization**: Uses a "Bounded Buffer" to ensure producers don't add to a full buffer and consumers don't take from an empty one.

---

## 8. Thread Pool
Maintains a pool of worker threads. Tasks are submitted to a queue and picked up by available threads.
- **Benefit**: Reduces the overhead of thread creation/destruction and prevents the system from being overwhelmed by too many concurrent threads.

---

## 9. Future and Promise
Synchronization constructs used to represent the result of an asynchronous operation that may not have completed yet.
- **Promise**: A writable placeholder for the result.
- **Future**: A read-only view of the result, allowing the caller to wait for or check the status of the operation.
