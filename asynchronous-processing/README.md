# ⏳ Asynchronous Processing Patterns

Asynchronous processing allows a system to remain responsive by offloading long-running tasks to the background. This is essential for improving user experience and system throughput.

---

## 🗺️ Table of Contents
1. [Fire and Forget](#1-fire-and-forget)
2. [Callback Pattern (Webhooks)](#2-callback-pattern-webhooks)
3. [Polling Pattern](#3-polling-pattern)
4. [Fork-Join Pattern](#4-fork-join-pattern)
5. [Competing Consumers](#5-competing-consumers)

---

## 1. Fire and Forget
The client sends a request and doesn't wait for any response or confirmation that the task has started.
- **Best for**: Logging, analytics, or tasks where the outcome doesn't affect the immediate user flow.
- **Implementation**: Message queues or background threads.

---

## 2. Callback Pattern (Webhooks)
The client provides a URL (webhook) when initiating a long-running task. Once the task is complete, the server makes an HTTP request to that URL with the results.
- **Pros**: Client doesn't need to stay connected or poll.
- **Cons**: Client must expose a public endpoint and handle incoming security/validation.

---

## 3. Polling Pattern
The client initiates a task and receives a `JobID` or `TaskID`. The client then periodically sends requests to check the status of that ID.
- **Workflow**: 
    1. POST `/start-task` -> `202 Accepted { id: 123 }`
    2. GET `/status/123` -> `{ status: "pending" }`
    3. GET `/status/123` -> `{ status: "completed", result: [...] }`
- **Short Polling**: Fixed interval requests.
- **Long Polling**: Server holds the request open until data is available or a timeout occurs.

---

## 4. Fork-Join Pattern
Used to speed up processing by splitting a large task into smaller independent sub-tasks (Fork) and then combining their results (Join).
- **Mechanism**: Commonly used in parallel computing and multi-core processing.

---

## 5. Competing Consumers
Enables multiple concurrent consumers to process messages received on the same messaging channel.
- **Benefit**: Increases throughput and improves scalability and availability.
- **Constraint**: Messages must be able to be processed independently.

---

## ⚖️ Pattern Comparison

| Pattern | Client Complexity | Server Complexity | Efficiency |
| :--- | :--- | :--- | :--- |
| **Fire & Forget** | Low | Low | ✅ High |
| **Callback** | High | Medium | ✅ High |
| **Polling** | Medium | Low | 🐢 Low |
| **Fork-Join** | Low | High | ✅ High |
