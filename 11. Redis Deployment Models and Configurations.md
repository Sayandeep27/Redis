# Redis Deployment Models / Configurations

---

## Overview

Redis can operate in several configurations depending on your durability, performance, and scaling needs. At its core, Redis stays lightweight — even its clustering approach is intentionally minimal so that you decide how to distribute your data.

---

# 1. Single‑Node Setup (The simplest form)

A standalone Redis server. One machine, no replicas.

All reads/writes go to one server.

Easiest to run.

No failover.

```
        +-------------------+
        |   Redis (Single)  |
        |        Main       |
        +-------------------+
```

### Explanation

In a single‑node setup, Redis runs on a single machine and handles all operations directly.

* There are **no replicas**.
* There is **no redundancy**.
* If the Redis instance goes down, the system temporarily loses access to cached data.

### Characteristics

| Feature               | Description   |
| --------------------- | ------------- |
| Deployment Complexity | Very Easy     |
| Infrastructure        | Single server |
| Read Handling         | Same server   |
| Write Handling        | Same server   |
| Fault Tolerance       | None          |
| Failover              | Not available |

### Typical Use Cases

* Local development
* Prototyping
* Small applications
* Basic caching

---

# 2. Replication / High Availability Setup

Here, a main instance is paired with replicas.

Replicas synchronize from the main node and can take over during failures.

```
        +----------------+
        |   Main         |
        |   (Writer)     |
        +----------------+
                |
                v
        +----------------+
        |   Replica      |
        |   (Read‑Only)  |
        +----------------+
```

Main handles writes.

Replicas can take traffic for reads.

Provides fault tolerance but not horizontal scaling for writes.

---

## How Replication Works

1. A **primary Redis node** (Main) handles write operations.
2. One or more **replica nodes** copy data from the primary.
3. Replicas continuously synchronize data from the main instance.
4. Applications can send read requests to replicas.

---

## Architecture Flow

```
Client
  |
  v
Main Redis (Write Operations)
  |
  v
Replica Nodes (Read Operations)
```

---

## Responsibilities

### Main Node (Writer)

Handles:

* All **write operations**
* Data modifications
* Propagation of updates to replicas

Example:

```redis
SET user:1 "data"
INCR page_views
LPUSH queue job1
```

---

### Replica Nodes (Read‑Only)

Replica nodes:

* Synchronize data from the main node
* Serve **read queries**
* Reduce load on the primary server

Example:

```redis
GET user:1
GET page_views
LRANGE queue 0 -1
```

---

## Advantages of Replication

| Advantage         | Description                                     |
| ----------------- | ----------------------------------------------- |
| High Availability | If the main node fails, replicas can take over  |
| Read Scalability  | Read queries can be distributed across replicas |
| Fault Tolerance   | Data exists on multiple machines                |

---

## Limitation

Replication **does not provide horizontal scaling for writes**.

All write operations must still go through the **main node**.

| Operation Type | Scaling              |
| -------------- | -------------------- |
| Reads          | Scales with replicas |
| Writes         | Limited to main node |

---

## Example Production Architecture

```
              Application Servers
                      |
                      v
                Redis Main
                (Writes)
                      |
        -----------------------------
        |                           |
   Replica Node 1              Replica Node 2
      (Reads)                    (Reads)
```

---

## Summary

| Feature         | Single Node Setup | Replication Setup |
| --------------- | ----------------- | ----------------- |
| Servers         | 1                 | Multiple          |
| Complexity      | Very Low          | Moderate          |
| Read Scaling    | No                | Yes               |
| Write Scaling   | No                | No                |
| Failover        | No                | Possible          |
| Fault Tolerance | No                | Yes               |

---

## Key Takeaways

* **Single Node Setup** is the simplest Redis deployment.
* **Replication Setup** improves availability and read scalability.
* Replicas help distribute read traffic and provide redundancy.
* Writes are still handled by a single primary node.

---

**End of Document**
