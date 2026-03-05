# Redis Cluster — Simple Explanation

---

## Overview

A **Redis Cluster** is a distributed setup of Redis that allows data to be stored across **multiple Redis servers (nodes)** instead of a single machine.

This helps achieve:

* Horizontal scalability
* High availability
* Fault tolerance
* Better performance for large applications

In simple terms:

> Redis Cluster splits your data across multiple Redis servers so that no single machine has to store everything.

---

# Architecture

A Redis Cluster typically consists of:

| Component     | Description                                |
| ------------- | ------------------------------------------ |
| Master Nodes  | Store actual data and handle writes        |
| Replica Nodes | Backup copies of masters for failover      |
| Hash Slots    | Logical partitions used to distribute data |
| Redis Client  | Connects to cluster and routes requests    |

---

# Key Concept: Hash Slots

Redis Cluster distributes data using **16,384 hash slots**.

Every key belongs to exactly **one hash slot**.

The slot is calculated using the following formula:

```text
slot = CRC16(key) % 16384
```

Example:

| Key     | Calculated Slot | Stored In |
| ------- | --------------- | --------- |
| user:1  | 2000            | Master A  |
| user:2  | 8000            | Master B  |
| order:5 | 12000           | Master C  |

---

# Slot Distribution Across Nodes

Each **master node** owns a range of slots.

Example cluster:

| Node     | Slot Range    |
| -------- | ------------- |
| Master A | 0 – 5460      |
| Master B | 5461 – 10922  |
| Master C | 10923 – 16383 |

This means:

* Keys whose slots fall within a node's range are stored in that node
* Data is automatically distributed

This mechanism is called **data sharding**.

---

# Masters and Replicas

Each master node can have one or more **replica nodes**.

Replicas maintain copies of the master's data.

Example structure:

```text
Master A
  └── Replica A

Master B
  └── Replica B

Master C
  └── Replica C
```

Purpose of replicas:

| Benefit           | Explanation                                 |
| ----------------- | ------------------------------------------- |
| High Availability | System continues running if a master fails  |
| Failover          | Replica is promoted to master automatically |
| Read Scaling      | Replicas can serve read traffic             |

---

# Client Request Flow

A Redis client maintains a **slot map** that tells it which node owns which slots.

Example slot map:

```text
0–5460 → Master A
5461–10922 → Master B
10923–16383 → Master C
```

### Example Request

```text
GET user:1
```

Steps:

1. Client calculates slot
2. Finds which node owns the slot
3. Sends request directly to that node

---

# Wrong Node Handling (Redirection)

If a client sends a request to the wrong node, Redis responds with a **MOVED redirect**.

Example:

```text
MOVED 8000 127.0.0.1:7002
```

Meaning:

* Slot `8000` belongs to another node
* Client should retry request there

After this, the client updates its slot map.

---

# Cluster Communication (Gossip Protocol)

Redis nodes communicate using a **lightweight gossip protocol**.

Nodes periodically exchange information about:

* Node health
* Slot ownership
* Failures

This helps the cluster automatically detect failures.

---

# Automatic Failover

If a **master node fails**:

1. Cluster detects failure
2. Replica is elected
3. Replica becomes new master
4. Cluster continues operating

Example:

```text
Before Failure

Master A
 └── Replica A

After Failure

Replica A → promoted to Master
```

---

# Example Redis Cluster Layout

```text
                Redis Client
             (Slot Map Cached)
                     |
     --------------------------------------
     |                  |                 |
  Master A           Master B          Master C
  (0–5460)          (5461–10922)       (10923–16383)
     |                  |                 |
  Replica A          Replica B          Replica C
```

---

# Advantages of Redis Cluster

| Advantage           | Explanation                         |
| ------------------- | ----------------------------------- |
| Horizontal Scaling  | Add more nodes to increase capacity |
| High Availability   | Automatic failover using replicas   |
| Distributed Storage | Data spread across many nodes       |
| High Performance    | Parallel processing across nodes    |

---

# Limitations of Redis Cluster

Redis Cluster is intentionally **simple** and does not support some complex operations.

Not ideal for:

| Limitation                        | Explanation                   |
| --------------------------------- | ----------------------------- |
| Multi-key operations across nodes | Keys must belong to same slot |
| Complex transactions              | Limited support               |
| SQL-like joins                    | Redis is a key-value store    |
| Large automatic rebalancing       | Data movement can be manual   |

Important rule:

> All keys used in a command should belong to the **same hash slot**.

---

# When to Use Redis Cluster

Redis Cluster is useful when:

* Data size is too large for a single machine
* Application requires high availability
* Traffic is very high
* Distributed caching is required

Common use cases:

| Use Case            | Example                    |
| ------------------- | -------------------------- |
| Distributed caching | Web applications           |
| Session storage     | Login sessions             |
| Feature store       | Machine learning pipelines |
| Real-time analytics | Metrics and events         |

---

# Simple One-Line Summary

```text
Redis Cluster = Multiple Redis nodes that distribute data using 16,384 hash slots with master-replica architecture for scalability and high availability.
```

---

# Key Takeaways

* Redis Cluster splits data using **16,384 hash slots**
* Each **master owns a range of slots**
* **Replicas provide failover** and reliability
* Clients use a **slot map** to route requests
* Cluster automatically handles **node failures**

---

# End of Document
