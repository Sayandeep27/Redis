# Redis Pub/Sub & Streams Commands – Complete Guide

## Overview

Redis provides two powerful messaging systems:

| Feature                         | Description                                               |
| ------------------------------- | --------------------------------------------------------- |
| **Pub/Sub (Publish–Subscribe)** | Real‑time messaging between publishers and subscribers    |
| **Streams**                     | Persistent log‑based messaging system for event streaming |

Both systems are widely used for:

* Event-driven architectures
* Microservices communication
* Real-time notifications
* Message queues
* Data pipelines

---

# Redis Pub/Sub

Redis **Publish–Subscribe** allows messages to be broadcast to multiple subscribers instantly.

### Basic Flow

1. **Publisher** sends message to a channel
2. **Subscribers** listen to that channel
3. All subscribers receive the message

Example:

```
SUBSCRIBE news
PUBLISH news "New update available"
```

---

# Pub/Sub Commands

| Command      | Description                | Example                |
| ------------ | -------------------------- | ---------------------- |
| PUBLISH      | Publish message to channel | `PUBLISH news "hello"` |
| SUBSCRIBE    | Subscribe to channel       | `SUBSCRIBE news`       |
| UNSUBSCRIBE  | Unsubscribe from channel   | `UNSUBSCRIBE news`     |
| PSUBSCRIBE   | Subscribe using pattern    | `PSUBSCRIBE news.*`    |
| PUNSUBSCRIBE | Unsubscribe from pattern   | `PUNSUBSCRIBE news.*`  |

---

# Sharded Pub/Sub (Redis 7+)

| Command      | Description                    |
| ------------ | ------------------------------ |
| SPUBLISH     | Publish to shard channel       |
| SSUBSCRIBE   | Subscribe to shard channel     |
| SUNSUBSCRIBE | Unsubscribe from shard channel |

---

# Pub/Sub Introspection Commands

| Command              | Description                       |
| -------------------- | --------------------------------- |
| PUBSUB CHANNELS      | List active channels              |
| PUBSUB NUMSUB        | Number of subscribers per channel |
| PUBSUB NUMPAT        | Number of pattern subscriptions   |
| PUBSUB SHARDCHANNELS | List shard channels               |
| PUBSUB SHARDNUMSUB   | Subscribers in shard channels     |

---

# Redis Streams

Redis **Streams** are append‑only log structures used for **event streaming and message queues**.

Streams allow:

* Persistent messages
* Consumer groups
* Message replay
* Fault tolerance

Example:

```
XADD mystream * user Alice action login
```

---

# Stream Message Commands

| Command   | Description              | Example                         |
| --------- | ------------------------ | ------------------------------- |
| XADD      | Add entry to stream      | `XADD mystream * field value`   |
| XDEL      | Delete entry from stream | `XDEL mystream 1650000000000-0` |
| XLEN      | Length of stream         | `XLEN mystream`                 |
| XRANGE    | Range of entries         | `XRANGE mystream - +`           |
| XREVRANGE | Reverse range query      | `XREVRANGE mystream + -`        |

---

# Reading Streams

| Command    | Description               | Example                                     |
| ---------- | ------------------------- | ------------------------------------------- |
| XREAD      | Read from stream          | `XREAD STREAMS mystream 0`                  |
| XREADGROUP | Read using consumer group | `XREADGROUP GROUP g1 c1 STREAMS mystream >` |

---

# Consumer Group Management

| Command               | Description                 |
| --------------------- | --------------------------- |
| XGROUP CREATE         | Create consumer group       |
| XGROUP CREATECONSUMER | Create consumer             |
| XGROUP DELCONSUMER    | Delete consumer             |
| XGROUP DESTROY        | Delete group                |
| XGROUP SETID          | Set group last delivered ID |

Example:

```
XGROUP CREATE mystream group1 0
```

---

# Message Acknowledgement

| Command | Description                    |
| ------- | ------------------------------ |
| XACK    | Acknowledge message processing |

Example:

```
XACK mystream group1 1650000000000-0
```

---

# Pending Message Management

| Command    | Description                       |
| ---------- | --------------------------------- |
| XPENDING   | List pending messages             |
| XCLAIM     | Transfer message ownership        |
| XAUTOCLAIM | Automatically claim idle messages |

---

# Stream Metadata Commands

| Command         | Description         |
| --------------- | ------------------- |
| XINFO STREAM    | Stream information  |
| XINFO GROUPS    | Consumer group info |
| XINFO CONSUMERS | Consumer details    |

---

# Stream Maintenance

| Command | Description        |
| ------- | ------------------ |
| XTRIM   | Trim stream length |
| XSETID  | Set stream last ID |

---

# Complete Redis Pub/Sub Command Reference

| Command              |
| -------------------- |
| PUBLISH              |
| SUBSCRIBE            |
| UNSUBSCRIBE          |
| PSUBSCRIBE           |
| PUNSUBSCRIBE         |
| SPUBLISH             |
| SSUBSCRIBE           |
| SUNSUBSCRIBE         |
| PUBSUB CHANNELS      |
| PUBSUB NUMSUB        |
| PUBSUB NUMPAT        |
| PUBSUB SHARDCHANNELS |
| PUBSUB SHARDNUMSUB   |

---

# Complete Redis Streams Command Reference

| Command               |
| --------------------- |
| XACK                  |
| XADD                  |
| XAUTOCLAIM            |
| XCLAIM                |
| XDEL                  |
| XGROUP CREATE         |
| XGROUP CREATECONSUMER |
| XGROUP DELCONSUMER    |
| XGROUP DESTROY        |
| XGROUP SETID          |
| XINFO CONSUMERS       |
| XINFO GROUPS          |
| XINFO STREAM          |
| XLEN                  |
| XPENDING              |
| XRANGE                |
| XREAD                 |
| XREADGROUP            |
| XREVRANGE             |
| XSETID                |
| XTRIM                 |

---

# Real World Examples

## Real-Time Chat System

```
SUBSCRIBE chatroom
PUBLISH chatroom "Hello everyone"
```

---

## Event Streaming Pipeline

```
XADD events * type login user alice
```

---

## Consumer Group Processing

```
XGROUP CREATE events workers 0
XREADGROUP GROUP workers worker1 STREAMS events >
XACK events workers 1650000000000-0
```

---

# Redis CLI Example

```
redis-cli

SUBSCRIBE updates
PUBLISH updates "system update"

XADD mystream * sensor temp
XRANGE mystream - +
```

---

# Best Practices

* Use **Pub/Sub for real-time ephemeral messaging**.
* Use **Streams for persistent message queues**.
* Use **consumer groups** for scalable processing.
* Trim streams using **XTRIM** to control memory usage.

---

# Conclusion

Redis Pub/Sub and Streams provide powerful messaging capabilities for distributed systems.

They enable:

* Real-time communication
* Event-driven architectures
* Reliable message processing
* High-throughput data pipelines

Mastering these commands is essential for building **scalable real-time systems using Redis**.
