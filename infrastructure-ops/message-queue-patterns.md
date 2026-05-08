# 📨 Message Queue Patterns

Message queues and event brokers decouple services, allowing them to communicate asynchronously. This improves scalability, fault tolerance, and responsiveness.

---

## 🗺️ Table of Contents
1. [Key Messaging Patterns](#1-key-messaging-patterns)
2. [Message Broker Paradigms](#2-message-broker-paradigms)
3. [RabbitMQ vs. Apache Kafka](#3-rabbitmq-vs-apache-kafka)
4. [Reliability & Error Handling](#4-reliability--error-handling)

---

## 1. Key Messaging Patterns

### Point-to-Point (Queue)
A message is sent to a queue and is consumed by exactly **one** receiver. Even if multiple consumers are listening to the queue, the message broker ensures only one gets it.
- **Use Case**: Task distribution (e.g., image processing, sending emails).

### Publish/Subscribe (Pub/Sub)
A message is published to a topic. **Every** consumer that has subscribed to that topic receives a copy of the message.
- **Use Case**: Broadcasting events (e.g., "Order Placed" needs to notify the Inventory, Shipping, and Billing services).

### Competing Consumers
Multiple consumer instances listen to the same queue. The broker distributes messages among them, allowing you to scale out processing concurrently.
- **Use Case**: Handling high loads of background jobs.

### Request-Reply
Messaging is inherently asynchronous, but sometimes you need a response. The sender includes a `reply-to` queue name and a `correlation-id` in the message. The receiver processes the message and sends the result to the `reply-to` queue with the same `correlation-id`.
- **Use Case**: RPC (Remote Procedure Call) over message queues.

---

## 2. Message Broker Paradigms

### Smart Broker / Dumb Consumer (e.g., RabbitMQ, ActiveMQ)
The broker is responsible for keeping track of the state of messages (who has consumed what).
- When a consumer acknowledges a message, the broker deletes it from the queue.
- Complex routing logic (exchanges, bindings) happens within the broker itself.

### Dumb Broker / Smart Consumer (e.g., Apache Kafka, Amazon Kinesis)
The broker acts essentially as a distributed append-only log. It simply stores messages for a configured retention period.
- The broker does *not* track message state per consumer.
- Consumers are responsible for tracking their own position in the log (called an `offset`).
- Messages are not deleted when consumed; they are deleted only when the retention period expires.

---

## 3. RabbitMQ vs. Apache Kafka

While both facilitate asynchronous communication, they are built for fundamentally different use cases.

| Feature | RabbitMQ | Apache Kafka |
| :--- | :--- | :--- |
| **Primary Paradigm** | Traditional Message Queue | Distributed Event Streaming |
| **Routing** | Highly flexible (Direct, Topic, Fanout exchanges). | Simple (Messages go to Topics/Partitions based on a key). |
| **Message Ordering** | Guaranteed within a single queue. | Guaranteed within a single Partition. |
| **Retention** | Messages are deleted after acknowledgment. | Messages are retained by time or size (replayable). |
| **Throughput** | Tens of thousands of msgs/sec. | Millions of msgs/sec. |
| **Best For** | Complex routing, long-running tasks, RPC. | High throughput, event sourcing, stream processing. |

---

## 4. Reliability & Error Handling

### Dead Letter Queues (DLQ)
If a message cannot be processed successfully after a certain number of retries, it is moved to a special queue called a Dead Letter Queue.
- This prevents "poison messages" from blocking the processing of valid messages.
- Operators can monitor the DLQ, inspect the failed messages, fix the underlying bug, and optionally replay them.

### Consumer Acknowledgments (ACKs)
Consumers must acknowledge they have successfully processed a message.
- **Auto-ACK**: The broker considers the message processed as soon as it's sent. Risky, as crashes lead to data loss.
- **Manual-ACK**: The consumer explicitly tells the broker when processing is finished. If the consumer crashes before ACK-ing, the broker will re-deliver the message to another consumer.

### Exactly-Once Semantics
Achieving exactly-once delivery is notoriously difficult in distributed systems. Usually, brokers guarantee **At-Least-Once** delivery (requiring consumers to be idempotent). Kafka supports Exactly-Once semantics within its ecosystem using Transactional Producers.

---

> [!TIP]
> For a deep dive into Kafka-specific architectures and tuning, see the [Kafka Patterns guide](../kafka-patterns/README.md).

---

[⬅️ Back to Infrastructure & Ops](./README.md)
