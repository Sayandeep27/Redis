# Redis Hash Commands – Complete Guide

## Overview

Redis **Hashes** are data structures used to store **field-value pairs** inside a single key. They are very similar to objects or dictionaries in programming languages.

A Redis hash is commonly used to represent **objects such as users, products, sessions, and configurations**.

Example structure:

```
user:1001
  name -> John
  age -> 25
  city -> London
```

Advantages:

* Memory efficient
* Perfect for structured data
* Fast field level operations

---

# Basic Hash Commands

| Command | Description               | Example                         |
| ------- | ------------------------- | ------------------------------- |
| HSET    | Set field in hash         | `HSET user:1 name "John"`       |
| HGET    | Get value of field        | `HGET user:1 name`              |
| HMSET   | Set multiple fields       | `HMSET user:1 name John age 25` |
| HMGET   | Get multiple fields       | `HMGET user:1 name age`         |
| HGETALL | Get all fields and values | `HGETALL user:1`                |

---

# Field Existence and Deletion

| Command | Description               | Example               |
| ------- | ------------------------- | --------------------- |
| HEXISTS | Check if field exists     | `HEXISTS user:1 name` |
| HDEL    | Delete one or more fields | `HDEL user:1 age`     |

---

# Numeric Operations

| Command      | Description             | Example                            |
| ------------ | ----------------------- | ---------------------------------- |
| HINCRBY      | Increment integer value | `HINCRBY user:1 login_count 1`     |
| HINCRBYFLOAT | Increment float value   | `HINCRBYFLOAT product:1 price 2.5` |

---

# Retrieving Hash Information

| Command | Description      | Example        |
| ------- | ---------------- | -------------- |
| HKEYS   | Get all fields   | `HKEYS user:1` |
| HVALS   | Get all values   | `HVALS user:1` |
| HLEN    | Number of fields | `HLEN user:1`  |

---

# Random Field Commands

| Command               | Description                | Example                        |
| --------------------- | -------------------------- | ------------------------------ |
| HRANDFIELD            | Get random field           | `HRANDFIELD user:1`            |
| HRANDFIELD WITHVALUES | Get random field and value | `HRANDFIELD user:1 WITHVALUES` |

---

# String Length

| Command | Description           | Example               |
| ------- | --------------------- | --------------------- |
| HSTRLEN | Length of field value | `HSTRLEN user:1 name` |

---

# Iterating Over Hash

| Command | Description                | Example          |
| ------- | -------------------------- | ---------------- |
| HSCAN   | Incrementally iterate hash | `HSCAN user:1 0` |

---

# Complete Command Reference

| Command      |
| ------------ |
| HSET         |
| HSETNX       |
| HGET         |
| HMSET        |
| HMGET        |
| HGETALL      |
| HEXISTS      |
| HDEL         |
| HLEN         |
| HKEYS        |
| HVALS        |
| HINCRBY      |
| HINCRBYFLOAT |
| HSTRLEN      |
| HRANDFIELD   |
| HSCAN        |

---

# Real World Use Cases

## User Profile Storage

```
HSET user:1001 name "John" age 25 country "UK"
```

## Product Catalog

```
HSET product:1 name "Laptop" price 800 stock 25
```

## Login Counters

```
HINCRBY user:1001 login_count 1
```

---

# Redis CLI Example

```
redis-cli

HSET user:1 name "Alice" age 30
HGET user:1 name
HGETALL user:1
```

---

# Best Practices

* Use hashes when storing **multiple related fields**.
* Keep field names short to save memory.
* Prefer **HSCAN** over **HGETALL** for very large hashes.

---

# Conclusion

Redis hashes are extremely efficient for storing structured data and are widely used in:

* User profile storage
* Product information
* Session metadata
* Counters and analytics

Hashes allow **fast read/write operations on individual fields without retrieving the entire object**.
