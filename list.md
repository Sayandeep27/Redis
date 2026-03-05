# Redis List Commands – Complete Guide

## Overview

Redis **Lists** are ordered collections of strings. Elements are inserted at the **head (left)** or **tail (right)** of the list.

Redis lists are implemented as **linked lists**, which makes insertion operations extremely fast.

Lists are widely used for:

* Message queues
* Task queues
* Activity feeds
* Streaming logs
* Background job processing

Example:

```
LPUSH tasks "task1" "task2" "task3"
```

---

# Key Characteristics of Redis Lists

| Feature                  | Description                            |
| ------------------------ | -------------------------------------- |
| Ordered                  | Elements maintain insertion order      |
| Duplicate values allowed | Same element can appear multiple times |
| Fast insertions          | O(1) push/pop operations               |
| Flexible operations      | Access elements by index or range      |

---

# Basic Push Commands

| Command | Description                      | Example              |
| ------- | -------------------------------- | -------------------- |
| LPUSH   | Insert element at head of list   | `LPUSH tasks task1`  |
| RPUSH   | Insert element at tail of list   | `RPUSH tasks task2`  |
| LPUSHX  | Push to head only if list exists | `LPUSHX tasks task1` |
| RPUSHX  | Push to tail only if list exists | `RPUSHX tasks task2` |

---

# Pop Commands

| Command | Description              | Example      |
| ------- | ------------------------ | ------------ |
| LPOP    | Remove element from head | `LPOP tasks` |
| RPOP    | Remove element from tail | `RPOP tasks` |

---

# Blocking Pop Commands

These commands wait until an element becomes available.

| Command | Description        | Example         |
| ------- | ------------------ | --------------- |
| BLPOP   | Blocking left pop  | `BLPOP tasks 0` |
| BRPOP   | Blocking right pop | `BRPOP tasks 0` |

---

# Move Elements Between Lists

| Command    | Description                            | Example                                |
| ---------- | -------------------------------------- | -------------------------------------- |
| RPOPLPUSH  | Pop from tail and push to another list | `RPOPLPUSH queue processing`           |
| BRPOPLPUSH | Blocking version of RPOPLPUSH          | `BRPOPLPUSH queue processing 0`        |
| LMOVE      | Move element between lists             | `LMOVE queue processing LEFT RIGHT`    |
| BLMOVE     | Blocking LMOVE                         | `BLMOVE queue processing LEFT RIGHT 0` |

---

# Multi List Pop Commands (Redis 7+)

| Command | Description                     | Example                               |
| ------- | ------------------------------- | ------------------------------------- |
| LMPOP   | Pop element from multiple lists | `LMPOP 2 list1 list2 LEFT COUNT 2`    |
| BLMPOP  | Blocking LMPOP                  | `BLMPOP 0 2 list1 list2 LEFT COUNT 1` |

---

# List Inspection Commands

| Command | Description             | Example             |
| ------- | ----------------------- | ------------------- |
| LLEN    | Length of list          | `LLEN tasks`        |
| LRANGE  | Get elements in range   | `LRANGE tasks 0 -1` |
| LINDEX  | Get element by index    | `LINDEX tasks 0`    |
| LPOS    | Get position of element | `LPOS tasks task1`  |

---

# Updating List Elements

| Command | Description                  | Example                            |
| ------- | ---------------------------- | ---------------------------------- |
| LSET    | Set value at index           | `LSET tasks 0 newtask`             |
| LINSERT | Insert before or after pivot | `LINSERT tasks BEFORE task2 task1` |

---

# Removing Elements

| Command | Description        | Example              |
| ------- | ------------------ | -------------------- |
| LREM    | Remove elements    | `LREM tasks 1 task1` |
| LTRIM   | Trim list to range | `LTRIM tasks 0 10`   |

---

# Iterating Lists

Lists are typically accessed using **LRANGE**.

Example:

```
LRANGE tasks 0 -1
```

---

# Complete Redis List Command Reference

| Command    | Description                            |
| ---------- | -------------------------------------- |
| BLMOVE     | Blocking move between lists            |
| BLMPOP     | Blocking pop from multiple lists       |
| BLPOP      | Blocking left pop                      |
| BRPOP      | Blocking right pop                     |
| BRPOPLPUSH | Blocking pop and push                  |
| LINDEX     | Get element by index                   |
| LINSERT    | Insert element before/after pivot      |
| LLEN       | Get list length                        |
| LMOVE      | Move element between lists             |
| LMPOP      | Pop from multiple lists                |
| LPOP       | Pop element from head                  |
| LPOS       | Get element position                   |
| LPUSH      | Push element to head                   |
| LPUSHX     | Push to head if list exists            |
| LRANGE     | Get range of elements                  |
| LREM       | Remove elements                        |
| LSET       | Set value at index                     |
| LTRIM      | Trim list                              |
| RPOP       | Pop element from tail                  |
| RPOPLPUSH  | Pop from tail and push to another list |
| RPUSH      | Push element to tail                   |
| RPUSHX     | Push to tail if list exists            |

---

# Real World Examples

## Task Queue

```
LPUSH task_queue "task1"
LPUSH task_queue "task2"
BRPOP task_queue 0
```

---

## Message Processing Pipeline

```
RPOPLPUSH queue processing
```

---

## Activity Feed

```
LPUSH user:1:feed "post1"
LPUSH user:1:feed "post2"
LRANGE user:1:feed 0 10
```

---

# Redis CLI Example

```
redis-cli

LPUSH numbers 1 2 3
RPUSH numbers 4
LRANGE numbers 0 -1
LPOP numbers
LLEN numbers
```

---

# Best Practices

* Use lists for **queues and ordered logs**.
* Prefer **blocking commands (BLPOP, BRPOP)** for worker systems.
* Use **LTRIM** to maintain fixed-length logs.

---

# Conclusion

Redis Lists provide extremely efficient operations for handling ordered data and queues.

They are heavily used in:

* Background job processing
* Event streaming
* Task scheduling
* Log processing

Understanding Redis List commands is essential for building **scalable backend systems, message queues, and distributed processing pipelines**.
