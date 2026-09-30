# CockroachDB Architecture Summary

### 1. The Core Problem

* **Traditional RDBMS (PostgreSQL):** Relies on a single primary for writes. As data grows, manual application-level sharding and failover management (e.g., handling replication lag or split-brain scenarios) become operational nightmares.
* **The Distributed Challenge:** Databases must reliably determine causal ordering ("what happened first?"). Across different machines, clock drift makes global ordering difficult.

---

### 2. Spanner vs. CockroachDB

* **Google Spanner:** Solves time synchronization using specialized hardware (atomic clocks and GPS receivers) with its TrueTime API, intentionally pausing writes for a few milliseconds to ensure global ordering.
* **CockroachDB:** Built to achieve Spanner-like global consistency on standard commodity hardware using pure software, aiming to serve as a horizontally scalable alternative to PostgreSQL.

---

### 3. Core Architecture

* **Global Sorted Key-Value Map:** Eliminates traditional tables at the storage layer; all tables, rows, indexes, and metadata are represented as keys in a single ordered ledger.
* **Dynamic Ranges:** The key space is automatically split into ~512 MB contiguous chunks called *ranges*. As data grows, ranges split, rebalance, and migrate automatically in the background without manual sharding.
* **Multi-Raft Consensus:** Every range is replicated across nodes (typically 3 replicas) running its own independent Raft consensus group:
  * **Writes:** Require a quorum (majority).
  * **Reads:** Served directly without consensus coordination by the range's *leaseholder*.
  * **Failure Recovery:** If a node dies, affected ranges hold fast, localized micro-elections without full-cluster failover procedures.
* **Time Without Atomic Clocks:** Uses **Hybrid Logical Clocks (HLC)** to maintain causality by combining physical time (NTP) with logical counters. Instead of pausing every write, CockroachDB only incurs latency via retries when transactions collide within a clock uncertainty window.

---

### 4. Trade-offs

* **Physics Tax:** Higher write latency due to multi-node network hops and consensus compared to a local single-node PostgreSQL disk write.
* **Application Retries:** Defaults to strict serializable isolation, requiring applications to handle transaction retry errors.
* **PostgreSQL Compatibility:** Implements the Postgres wire protocol and SQL dialect (built from scratch in Go on the Pebble storage engine), but must continually chase PostgreSQL's evolving feature set and extensions.
* **Licensing:** Uses a source-available commercial model rather than traditional pure open source.


Source: https://www.youtube.com/watch?v=jxso9wTBkNg