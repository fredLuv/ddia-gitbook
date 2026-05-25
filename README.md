# Designing Data-Intensive Applications (DDIA) Study GitBook

Welcome to the ultimate offline study guide and reference GitBook for Martin Kleppmann's landmark book, **"Designing Data-Intensive Applications" (O'Reilly)**. 

Modern applications are **data-intensive**, meaning their primary challenges are the amount of data, the complexity of data, and the speed at which it changes, rather than raw CPU processing limits. This GitBook provides exhaustive chapter-by-chapter breakdowns, core diagrams, system trade-offs, and practical design cheat sheets to help you build reliable, scalable, and maintainable systems.

---

## 🗺️ Book Structure

The book is divided into three core parts:

### [Part I: Foundations of Data Systems](chapters/ch1_foundations.md)
We explore the fundamental building blocks of data-intensive systems, including design parameters (Reliability, Scalability, Maintainability), query models, internal storage architectures (LSM-trees vs. B-trees), and serialization formats.

### [Part II: Distributed Data](chapters/ch5_replication.md)
We transition from a single database node to distributed systems. We detail how to replicate data, partition (shard) massive datasets across nodes, guarantee transactional safety, deal with network and clock failures, and achieve distributed consensus.

### [Part III: Derived Data](chapters/ch10_batch.md)
We discuss how to integrate separate data systems to form complete architectures. We explore batch processing systems (MapReduce, Spark), real-time stream processing systems (Kafka, Flink), and mechanisms for ensuring eventual correctness.

---

## 🔗 Navigation Index

Use the central **[Table of Contents / SUMMARY.md](SUMMARY.md)** to jump directly to any specific chapter, or browse through the individual chapters below:

*   **Part I: Foundations of Data Systems**
    1.  [Chapter 1: Reliable, Scalable, and Maintainable Applications](chapters/ch1_foundations.md)
    2.  [Chapter 2: Data Models and Query Languages](chapters/ch2_data_models.md)
    3.  [Chapter 3: Storage and Retrieval](chapters/ch3_storage.md)
    4.  [Chapter 4: Encoding and Evolution](chapters/ch4_encoding.md)
*   **Part II: Distributed Data**
    5.  [Chapter 5: Replication](chapters/ch5_replication.md)
    6.  [Chapter 6: Partitioning](chapters/ch6_partitioning.md)
    7.  [Chapter 7: Transactions](chapters/ch7_transactions.md)
    8.  [Chapter 8: The Trouble with Distributed Systems](chapters/ch8_troubles.md)
    9.  [Chapter 9: Consistency and Consensus](chapters/ch9_consistency.md)
*   **Part III: Derived Data**
    10. [Chapter 10: Batch Processing](chapters/ch10_batch.md)
    11. [Chapter 11: Stream Processing](chapters/ch11_stream.md)
    12. [Chapter 12: The Future of Data Systems](chapters/ch12_future.md)
