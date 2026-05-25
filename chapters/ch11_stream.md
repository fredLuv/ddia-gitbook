# Chapter 11: Stream Processing

## 🎯 Core Thesis
Batch processing operates on static, bounded data. However, in the real world, data is a continuous, unbounded flow of events. **Stream Processing** is the real-time consumption and computation of this unbounded data. To process streams reliably and at scale, we must move beyond traditional message queues and adopt **Log-Based Message Brokers (like Apache Kafka)**, and design pipelines capable of handling out-of-order events, time drift, and stateful joins.

---

## 🔑 Key Terminology

*   **Stream**: Unbounded, continuously arriving data (e.g., user activity clicks, IoT sensor telemetry, database transaction updates).
*   **Producer**: An application that writes events to a stream.
*   **Consumer**: An application that reads and processes events from a stream.
*   **Topic**: A logical folder or channel in a message broker that groups related streams of events.
*   **Partition**: A physical slice of a Topic. Partitions allow topics to scale out across multiple nodes by sharding events based on a key.
*   **Log-Based Message Broker**: A broker (like Kafka) that structures topics as append-only log files on disk, allowing multiple consumers to read from different positions independently.
*   **Change Data Capture (CDC)**: The process of capturing all insert, update, and delete events written to a database's transaction log and streaming them to other systems (e.g., search indexes or caches) in real-time.
*   **Event Time**: The exact timestamp when an event originally occurred on the client device.
*   **Processing Time**: The timestamp when the event is processed by the stream processing engine.
*   **Windowing**: Grouping unbounded data into bounded slices of time (e.g., hourly, tumbling, sliding, or session windows) for aggregation.

---

## 📬 Message Brokers: Log-Based vs. AMQP

Unlike traditional message queues (like RabbitMQ/ActiveMQ) which delete messages once they are acknowledged, **Log-Based Message Brokers** retain messages on disk:

```text
Traditional Message Queue (AMQP/JMS):
[Broker Memory] ---> Msg1 (Consumed & Deleted) ---> Msg2 (In Progress)

Log-Based Message Broker (Kafka):
Disk Log:  [Msg0] -> [Msg1] -> [Msg2] -> [Msg3] -> [Msg4]
             ^                  ^
             |                  |
       Consumer Group A     Consumer Group B
       Offset: 1            Offset: 3
```

### Advantages of Log-Based Message Brokers:
1.  **Replayability**: Consumers can reset their **offset** to replay historical messages (e.g., if a bug is fixed and data needs to be recalculated).
2.  **Zero-Impact Scale**: Multiple independent consumer groups can read the same topic simultaneously at their own pace without affecting each other or consuming broker memory.
3.  **High Throughput**: By writing sequentially to disk partitions, Kafka achieves throughput comparable to memory-bound brokers.

---

## ⏰ Time Handling & Windowing

In stream processing, network latency and offline devices mean events can arrive out of order. Therefore, **Event Time** and **Processing Time** often diverge significantly.

> [!WARNING]
> Never use **Processing Time** to compute time-sensitive windowed statistics (e.g., counting requests per minute). If a network partition occurs and drops communication for 10 minutes, all delayed events will rush into the engine at once when the partition heals, skewing processing-time windows catastrophically.

### Windowing Strategies
*   **Tumbling Window**: Fixed-size, non-overlapping time windows (e.g., 5-minute blocks).
*   **Sliding Window**: Overlapping windows of a fixed size that slide over time (e.g., a 10-minute window that recalculates every 1 minute).
*   **Session Window**: Bounded by periods of inactivity (e.g., grouping user clicks together until they stop clicking for 30 minutes).

---

## 🔗 Stateful Stream Joins

Joining continuous streams raises unique state management challenges:

1.  **Stream-Stream Join (Event Correlation)**:
    *   *Example*: Joining `SearchClicks` and `PurchaseEvents` to measure ad conversion.
    *   *Challenge*: The engine must maintain a state hash map of all clicks and all purchases for a window of time (e.g., 1 hour), matching them as they arrive.
2.  **Stream-Table Join (Enrichment)**:
    *   *Example*: Enriching a raw stream of `UserTransactions` with their `UserProfile` database data.
    *   *Challenge*: The stream engine can query the database directly for every event (slow network bottleneck), or it can consume a **CDC stream** of user profile changes to maintain a local, in-memory replica of the profile table (fast local lookup).
3.  **Table-Table Join (Materialized View Maintenance)**:
    *   *Example*: Joining two database tables in real-time to maintain an aggregated, joined view. Both inputs are streams of changes (CDC), and the output is a continuous stream of updates.
