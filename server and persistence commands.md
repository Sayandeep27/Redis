# Redis Server & Persistence Commands – Complete Guide

## Overview

Redis provides a set of **Server Management** and **Persistence** commands that allow administrators and developers to control the Redis instance, monitor performance, and manage how data is saved to disk.

These commands are essential for:

* Database administration
* Monitoring Redis health
* Managing persistence (RDB & AOF)
* Debugging and troubleshooting
* Runtime configuration

---

# Redis Persistence Mechanisms

Redis supports two main persistence mechanisms:

| Persistence Type           | Description                        |
| -------------------------- | ---------------------------------- |
| **RDB (Snapshotting)**     | Periodically saves dataset to disk |
| **AOF (Append Only File)** | Logs every write operation         |

These persistence modes help Redis recover data after crashes or restarts.

---

# RDB Snapshot Commands

| Command  | Description                               | Example    |
| -------- | ----------------------------------------- | ---------- |
| SAVE     | Synchronously save dataset to disk        | `SAVE`     |
| BGSAVE   | Asynchronously save dataset in background | `BGSAVE`   |
| LASTSAVE | Timestamp of last successful save         | `LASTSAVE` |

---

# AOF Persistence Commands

| Command      | Description                            | Example          |
| ------------ | -------------------------------------- | ---------------- |
| BGREWRITEAOF | Rewrite Append Only File in background | `BGREWRITEAOF`   |
| WAITAOF      | Wait for AOF fsync propagation         | `WAITAOF 1 1000` |

---

# Server Information Commands

| Command | Description                | Example  |
| ------- | -------------------------- | -------- |
| INFO    | Get server statistics      | `INFO`   |
| DBSIZE  | Number of keys in database | `DBSIZE` |
| TIME    | Get server time            | `TIME`   |
| ROLE    | Get replication role       | `ROLE`   |

---

# Configuration Commands

| Command        | Description                 | Example                    |
| -------------- | --------------------------- | -------------------------- |
| CONFIG GET     | Get configuration parameter | `CONFIG GET maxmemory`     |
| CONFIG SET     | Set configuration parameter | `CONFIG SET maxmemory 1gb` |
| CONFIG REWRITE | Rewrite configuration file  | `CONFIG REWRITE`           |

---

# Database Management Commands

| Command  | Description                         | Example    |
| -------- | ----------------------------------- | ---------- |
| FLUSHDB  | Delete all keys in current database | `FLUSHDB`  |
| FLUSHALL | Delete all keys in all databases    | `FLUSHALL` |
| SHUTDOWN | Stop Redis server                   | `SHUTDOWN` |

---

# Monitoring Commands

| Command       | Description                | Example         |
| ------------- | -------------------------- | --------------- |
| MONITOR       | Stream real-time commands  | `MONITOR`       |
| SLOWLOG GET   | Get slow query log         | `SLOWLOG GET`   |
| SLOWLOG LEN   | Number of slow log entries | `SLOWLOG LEN`   |
| SLOWLOG RESET | Reset slow log             | `SLOWLOG RESET` |

---

# Latency Monitoring Commands

| Command           | Description               |
| ----------------- | ------------------------- |
| LATENCY DOCTOR    | Analyze latency issues    |
| LATENCY GRAPH     | Latency graph for event   |
| LATENCY HELP      | Help for latency commands |
| LATENCY HISTOGRAM | Latency distribution      |
| LATENCY LATEST    | Latest latency spikes     |
| LATENCY RESET     | Reset latency data        |

---

# Memory Management Commands

| Command             | Description              |
| ------------------- | ------------------------ |
| MEMORY DOCTOR       | Diagnose memory problems |
| MEMORY HELP         | Memory command help      |
| MEMORY MALLOC-STATS | Allocator statistics     |
| MEMORY PURGE        | Purge allocator memory   |
| MEMORY STATS        | Detailed memory stats    |
| MEMORY USAGE        | Memory usage of key      |

---

# Command Introspection

| Command                 | Description           |
| ----------------------- | --------------------- |
| COMMAND                 | List server commands  |
| COMMAND COUNT           | Number of commands    |
| COMMAND GETKEYS         | Extract key arguments |
| COMMAND GETKEYSANDFLAGS | Get keys with flags   |
| COMMAND INFO            | Command metadata      |
| COMMAND LIST            | List command names    |

---

# Replication & Synchronization Commands

| Command   | Description                             |
| --------- | --------------------------------------- |
| REPLICAOF | Configure replication source            |
| PSYNC     | Partial resynchronization               |
| REPLCONF  | Configure replication parameters        |
| WAIT      | Wait for replicas to acknowledge writes |

---

# Debug Commands

| Command      | Description                    |
| ------------ | ------------------------------ |
| DEBUG OBJECT | Inspect Redis object internals |

---

# Complete Redis Server & Persistence Command Reference

| Command                 |
| ----------------------- |
| BGREWRITEAOF            |
| BGSAVE                  |
| COMMAND                 |
| COMMAND COUNT           |
| COMMAND GETKEYS         |
| COMMAND GETKEYSANDFLAGS |
| COMMAND INFO            |
| COMMAND LIST            |
| CONFIG GET              |
| CONFIG REWRITE          |
| CONFIG SET              |
| DBSIZE                  |
| DEBUG OBJECT            |
| FLUSHALL                |
| FLUSHDB                 |
| INFO                    |
| LASTSAVE                |
| LATENCY DOCTOR          |
| LATENCY GRAPH           |
| LATENCY HELP            |
| LATENCY HISTOGRAM       |
| LATENCY LATEST          |
| LATENCY RESET           |
| MEMORY DOCTOR           |
| MEMORY HELP             |
| MEMORY MALLOC-STATS     |
| MEMORY PURGE            |
| MEMORY STATS            |
| MEMORY USAGE            |
| MONITOR                 |
| PSYNC                   |
| REPLCONF                |
| REPLICAOF               |
| ROLE                    |
| SAVE                    |
| SHUTDOWN                |
| SLOWLOG GET             |
| SLOWLOG LEN             |
| SLOWLOG RESET           |
| TIME                    |
| WAIT                    |
| WAITAOF                 |

---

# Redis CLI Example

```
redis-cli

INFO
DBSIZE
BGSAVE
LASTSAVE
CONFIG GET maxmemory
SLOWLOG GET
```

---

# Best Practices

* Use **BGSAVE instead of SAVE** in production environments.
* Monitor Redis using **INFO, SLOWLOG, and MEMORY STATS**.
* Use **AOF rewriting (BGREWRITEAOF)** to prevent large log files.
* Avoid running **FLUSHALL** in production unless necessary.

---

# Conclusion

Redis server and persistence commands are essential for **operating and maintaining Redis in production environments**.

They help manage:

* Server configuration
* Data durability
* Performance monitoring
* Replication and synchronization
* Memory usage

Mastering these commands is crucial for **DevOps, Backend Engineers, and MLOps professionals working with Redis infrastructure**.
