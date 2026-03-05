# Redis Sorted Set (ZSET) Commands – Complete Guide

## Overview

Redis **Sorted Sets (ZSETs)** are collections of **unique strings** where each member is associated with a **score** (a floating‑point number). The score is used to **automatically keep the set sorted**.

Sorted sets are extremely powerful for ranking systems and time‑based ordering.

Common use cases:

* Leaderboards
* Ranking systems
* Priority queues
* Time‑series ordering
* Recommendation systems

Example:

```
ZADD leaderboard 100 "player1" 200 "player2" 150 "player3"
```

---

# Key Characteristics of Sorted Sets

| Feature                | Description                                  |
| ---------------------- | -------------------------------------------- |
| Unique members         | Duplicate members are not allowed            |
| Score based ordering   | Each member has a numeric score              |
| Automatic sorting      | Members sorted by score                      |
| Fast operations        | O(log N) for most updates                    |
| Powerful range queries | Query by score, rank, or lexicographic order |

---

# Basic Sorted Set Commands

| Command | Description                    | Example                               |
| ------- | ------------------------------ | ------------------------------------- |
| ZADD    | Add members with scores        | `ZADD leaderboard 100 player1`        |
| ZCARD   | Number of elements             | `ZCARD leaderboard`                   |
| ZSCORE  | Get score of member            | `ZSCORE leaderboard player1`          |
| ZMSCORE | Get scores of multiple members | `ZMSCORE leaderboard player1 player2` |

---

# Incrementing Scores

| Command | Description               | Example                          |
| ------- | ------------------------- | -------------------------------- |
| ZINCRBY | Increment score of member | `ZINCRBY leaderboard 10 player1` |

---

# Removing Members

| Command          | Description                           | Example                              |
| ---------------- | ------------------------------------- | ------------------------------------ |
| ZREM             | Remove member(s)                      | `ZREM leaderboard player1`           |
| ZREMRANGEBYRANK  | Remove members by rank                | `ZREMRANGEBYRANK leaderboard 0 10`   |
| ZREMRANGEBYSCORE | Remove members by score               | `ZREMRANGEBYSCORE leaderboard 0 100` |
| ZREMRANGEBYLEX   | Remove members by lexicographic range | `ZREMRANGEBYLEX key [a [z`           |

---

# Rank Operations

| Command  | Description                | Example                        |
| -------- | -------------------------- | ------------------------------ |
| ZRANK    | Rank of member (ascending) | `ZRANK leaderboard player1`    |
| ZREVRANK | Rank in reverse order      | `ZREVRANK leaderboard player1` |

---

# Range Queries

## By Rank

| Command     | Description        | Example                               |
| ----------- | ------------------ | ------------------------------------- |
| ZRANGE      | Get range by index | `ZRANGE leaderboard 0 10`             |
| ZREVRANGE   | Reverse range      | `ZREVRANGE leaderboard 0 10`          |
| ZRANGESTORE | Store range result | `ZRANGESTORE result leaderboard 0 10` |

---

## By Score

| Command          | Description            | Example                              |
| ---------------- | ---------------------- | ------------------------------------ |
| ZRANGEBYSCORE    | Range by score         | `ZRANGEBYSCORE leaderboard 0 100`    |
| ZREVRANGEBYSCORE | Reverse range by score | `ZREVRANGEBYSCORE leaderboard 100 0` |

---

## By Lexicographic Order

| Command        | Description                  | Example                    |
| -------------- | ---------------------------- | -------------------------- |
| ZRANGEBYLEX    | Range by lexicographic order | `ZRANGEBYLEX key [a [z`    |
| ZREVRANGEBYLEX | Reverse lexicographic range  | `ZREVRANGEBYLEX key [z [a` |
| ZLEXCOUNT      | Count lexicographic range    | `ZLEXCOUNT key [a [z`      |

---

# Score Counting

| Command | Description                  | Example                    |
| ------- | ---------------------------- | -------------------------- |
| ZCOUNT  | Count members by score range | `ZCOUNT leaderboard 0 100` |

---

# Random Member Retrieval

| Command     | Description       | Example                   |
| ----------- | ----------------- | ------------------------- |
| ZRANDMEMBER | Get random member | `ZRANDMEMBER leaderboard` |

---

# Pop Operations

| Command | Description                      | Example               |
| ------- | -------------------------------- | --------------------- |
| ZPOPMAX | Remove member with highest score | `ZPOPMAX leaderboard` |
| ZPOPMIN | Remove member with lowest score  | `ZPOPMIN leaderboard` |

---

# Blocking Pop Commands

| Command  | Description      | Example                  |
| -------- | ---------------- | ------------------------ |
| BZPOPMAX | Blocking pop max | `BZPOPMAX leaderboard 0` |
| BZPOPMIN | Blocking pop min | `BZPOPMIN leaderboard 0` |

---

# Multi Sorted Set Pop

| Command | Description                     | Example                    |
| ------- | ------------------------------- | -------------------------- |
| ZMPOP   | Pop elements from multiple sets | `ZMPOP 2 set1 set2 MIN`    |
| BZMPOP  | Blocking ZMPOP                  | `BZMPOP 0 2 set1 set2 MIN` |

---

# Set Operations (Sorted Sets)

## Intersection

| Command     | Description                 | Example                          |
| ----------- | --------------------------- | -------------------------------- |
| ZINTER      | Intersection of sorted sets | `ZINTER 2 set1 set2`             |
| ZINTERSTORE | Store intersection          | `ZINTERSTORE result 2 set1 set2` |
| ZINTERCARD  | Cardinality of intersection | `ZINTERCARD 2 set1 set2`         |

---

## Union

| Command     | Description          | Example                          |
| ----------- | -------------------- | -------------------------------- |
| ZUNION      | Union of sorted sets | `ZUNION 2 set1 set2`             |
| ZUNIONSTORE | Store union          | `ZUNIONSTORE result 2 set1 set2` |

---

## Difference

| Command    | Description        | Example                         |
| ---------- | ------------------ | ------------------------------- |
| ZDIFF      | Difference of sets | `ZDIFF 2 set1 set2`             |
| ZDIFFSTORE | Store difference   | `ZDIFFSTORE result 2 set1 set2` |

---

# Iterating Sorted Sets

| Command | Description                      | Example               |
| ------- | -------------------------------- | --------------------- |
| ZSCAN   | Incrementally iterate sorted set | `ZSCAN leaderboard 0` |

---

# Complete Redis Sorted Set Command Reference

| Command          |
| ---------------- |
| ZADD             |
| ZCARD            |
| ZCOUNT           |
| ZDIFF            |
| ZDIFFSTORE       |
| ZINCRBY          |
| ZINTER           |
| ZINTERCARD       |
| ZINTERSTORE      |
| ZLEXCOUNT        |
| ZMPOP            |
| BZMPOP           |
| ZPOPMAX          |
| ZPOPMIN          |
| BZPOPMAX         |
| BZPOPMIN         |
| ZRANDMEMBER      |
| ZRANGE           |
| ZRANGEBYLEX      |
| ZRANGEBYSCORE    |
| ZRANGESTORE      |
| ZRANK            |
| ZREM             |
| ZREMRANGEBYLEX   |
| ZREMRANGEBYRANK  |
| ZREMRANGEBYSCORE |
| ZREVRANGE        |
| ZREVRANGEBYLEX   |
| ZREVRANGEBYSCORE |
| ZREVRANK         |
| ZSCAN            |
| ZSCORE           |
| ZMSCORE          |
| ZUNION           |
| ZUNIONSTORE      |

---

# Real World Examples

## Leaderboard System

```
ZADD leaderboard 100 player1
ZADD leaderboard 200 player2
ZREVRANGE leaderboard 0 10 WITHSCORES
```

---

## Priority Queue

```
ZADD tasks 1 "urgent_task"
ZADD tasks 5 "low_priority"
ZPOPMIN tasks
```

---

## Time Based Event Storage

```
ZADD events 1710000000 "login_event"
ZRANGEBYSCORE events 1700000000 1720000000
```

---

# Redis CLI Example

```
redis-cli

ZADD leaderboard 100 player1
ZADD leaderboard 200 player2
ZRANGE leaderboard 0 -1 WITHSCORES
ZINCRBY leaderboard 50 player1
ZREVRANGE leaderboard 0 10 WITHSCORES
```

---

# Best Practices

* Use sorted sets for **ranking and scoring systems**.
* Use **ZRANGE WITHSCORES** when scores are needed.
* Use **ZINCRBY** for leaderboard updates.
* Use **ZSCAN** instead of full scans for very large sets.

---

# Conclusion

Redis Sorted Sets combine the **uniqueness of sets** with the **ordering power of scores**.

They are widely used in:

* Leaderboards
* Ranking systems
* Time‑series data
* Scheduling systems
* Recommendation engines

Understanding sorted set commands is essential for building **high‑performance ranking and analytics systems with Redis**.
