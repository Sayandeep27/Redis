# Redis Set Commands – Complete Guide

## Overview

Redis **Sets** are an unordered collection of **unique strings**. This means:

* Duplicate values are **not allowed**
* Elements are **unordered**
* Operations such as **union, intersection, and difference** are very fast

Redis sets are commonly used for:

* Tag systems
* Unique visitors tracking
* Social network relationships
* Recommendation engines

Example:

```
SADD users "alice" "bob" "charlie"
```

---

# Key Characteristics of Redis Sets

| Feature                 | Description                                  |
| ----------------------- | -------------------------------------------- |
| Unique values           | Duplicate elements are automatically ignored |
| Unordered               | No guaranteed order of elements              |
| Fast operations         | Most commands are O(1)                       |
| Powerful set operations | Supports union, intersection, difference     |

---

# Basic Set Commands

| Command   | Description                      | Example                   |
| --------- | -------------------------------- | ------------------------- |
| SADD      | Add one or more members to a set | `SADD users "alice"`      |
| SREM      | Remove one or more members       | `SREM users "alice"`      |
| SMEMBERS  | Get all members in a set         | `SMEMBERS users`          |
| SCARD     | Get number of elements           | `SCARD users`             |
| SISMEMBER | Check if value exists            | `SISMEMBER users "alice"` |

---

# Multiple Member Operations

| Command    | Description            | Example                          |
| ---------- | ---------------------- | -------------------------------- |
| SMISMEMBER | Check multiple members | `SMISMEMBER users "alice" "bob"` |

---

# Random Element Operations

| Command     | Description                        | Example             |
| ----------- | ---------------------------------- | ------------------- |
| SPOP        | Remove and return random member    | `SPOP users`        |
| SRANDMEMBER | Get random member without removing | `SRANDMEMBER users` |

---

# Moving Elements Between Sets

| Command | Description                         | Example                            |
| ------- | ----------------------------------- | ---------------------------------- |
| SMOVE   | Move member from one set to another | `SMOVE users active_users "alice"` |

---

# Set Mathematical Operations

These commands perform mathematical operations between sets.

## Union

| Command     | Description          | Example                        |
| ----------- | -------------------- | ------------------------------ |
| SUNION      | Return union of sets | `SUNION set1 set2`             |
| SUNIONSTORE | Store union result   | `SUNIONSTORE result set1 set2` |

---

## Intersection

| Command     | Description                     | Example                        |
| ----------- | ------------------------------- | ------------------------------ |
| SINTER      | Return intersection             | `SINTER set1 set2`             |
| SINTERSTORE | Store intersection result       | `SINTERSTORE result set1 set2` |
| SINTERCARD  | Get cardinality of intersection | `SINTERCARD 2 set1 set2`       |

---

## Difference

| Command    | Description                    | Example                       |
| ---------- | ------------------------------ | ----------------------------- |
| SDIFF      | Return difference between sets | `SDIFF set1 set2`             |
| SDIFFSTORE | Store difference result        | `SDIFFSTORE result set1 set2` |

---

# Iterating Over Large Sets

| Command | Description                        | Example         |
| ------- | ---------------------------------- | --------------- |
| SSCAN   | Incrementally iterate set elements | `SSCAN users 0` |

---

# Complete Redis Set Command Reference

The following table lists **all Redis Set commands**.

| Command     | Description                     |
| ----------- | ------------------------------- |
| SADD        | Add members to a set            |
| SCARD       | Get number of elements in a set |
| SDIFF       | Difference between sets         |
| SDIFFSTORE  | Store difference of sets        |
| SINTER      | Intersection of sets            |
| SINTERCARD  | Cardinality of intersection     |
| SINTERSTORE | Store intersection result       |
| SISMEMBER   | Check if member exists          |
| SMISMEMBER  | Check multiple members          |
| SMEMBERS    | Return all members              |
| SMOVE       | Move member between sets        |
| SPOP        | Remove and return random member |
| SRANDMEMBER | Return random member            |
| SREM        | Remove members from set         |
| SSCAN       | Iterate through set             |
| SUNION      | Union of sets                   |
| SUNIONSTORE | Store union result              |

---

# Real World Examples

## Unique Website Visitors

```
SADD visitors user1 user2 user3
SCARD visitors
```

---

## Mutual Friends (Intersection)

```
SINTER user:1:friends user:2:friends
```

---

## Recommendation System

```
SUNION user:1:likes user:2:likes
```

---

## Remove Inactive Users

```
SDIFF active_users inactive_users
```

---

# Redis CLI Example

```
redis-cli

SADD fruits "apple" "banana" "orange"
SMEMBERS fruits
SISMEMBER fruits "apple"
SCARD fruits
```

---

# Best Practices

* Use sets when **uniqueness is required**.
* Use **set operations (SINTER, SUNION, SDIFF)** for analytics and recommendation systems.
* Use **SSCAN** instead of **SMEMBERS** for very large sets.

---

# Conclusion

Redis Sets provide powerful and extremely fast operations for handling unique collections of data.

They are widely used in:

* Social networks
* Recommendation engines
* Unique event tracking
* Tag systems
* Graph-like relationships

Understanding Redis Set commands is essential for **backend systems, distributed systems, and large-scale data applications**.
