# Redis String Commands – Complete Guide

## Overview

Redis **Strings** are the most basic and widely used data type. A Redis string can store:

* Text
* Numbers
* Binary data

Maximum size of a Redis string value: **512 MB**.

---

# Basic Commands

| Command | Description                  | Example            |
| ------- | ---------------------------- | ------------------ |
| SET     | Set value of a key           | `SET name "Alex"`  |
| GET     | Get value of key             | `GET name`         |
| MSET    | Set multiple keys            | `MSET a 1 b 2`     |
| MGET    | Get multiple keys            | `MGET a b`         |
| SETNX   | Set if not exists            | `SETNX key value`  |
| GETSET  | Set new value and return old | `GETSET key value` |

---

# Numeric Operations

| Command     | Description        | Example                 |
| ----------- | ------------------ | ----------------------- |
| INCR        | Increment integer  | `INCR counter`          |
| INCRBY      | Increment by value | `INCRBY counter 10`     |
| DECR        | Decrement integer  | `DECR counter`          |
| DECRBY      | Decrement by value | `DECRBY counter 5`      |
| INCRBYFLOAT | Increment float    | `INCRBYFLOAT price 1.5` |

---

# String Manipulation

| Command  | Description       | Example               |
| -------- | ----------------- | --------------------- |
| APPEND   | Append string     | `APPEND key "world"`  |
| STRLEN   | Length of string  | `STRLEN key`          |
| GETRANGE | Get substring     | `GETRANGE key 0 5`    |
| SETRANGE | Replace substring | `SETRANGE key 0 "Hi"` |

---

# Bit Operations

| Command  | Description       | Example              |
| -------- | ----------------- | -------------------- |
| SETBIT   | Set bit at offset | `SETBIT key 7 1`     |
| GETBIT   | Get bit value     | `GETBIT key 7`       |
| BITCOUNT | Count bits        | `BITCOUNT key`       |
| BITPOS   | Find bit position | `BITPOS key 1`       |
| BITOP    | Bitwise operation | `BITOP AND dest a b` |

---

# Advanced Commands

| Command | Description                     | Example                         |
| ------- | ------------------------------- | ------------------------------- |
| SETEX   | Set with expiration             | `SETEX key 60 value`            |
| PSETEX  | Set with millisecond expiration | `PSETEX key 1000 value`         |
| SETPX   | Set with PX option              | `SET key value PX 1000`         |
| SETEXAT | Set expiration at unix time     | `SET key value EXAT 1700000000` |

---

# Multi-Key Atomic Operations

| Command | Description                     | Example          |
| ------- | ------------------------------- | ---------------- |
| MSETNX  | Set multiple keys if none exist | `MSETNX a 1 b 2` |

---

# Conditional Set Options

| Option | Description                    |
| ------ | ------------------------------ |
| NX     | Only set if key does not exist |
| XX     | Only set if key exists         |
| EX     | Expire time in seconds         |
| PX     | Expire time in milliseconds    |

Example:

```
SET session:1 "user_data" EX 60 NX
```

---

# Real World Examples

## User Session Storage

```
SET session:user123 "logged_in" EX 3600
```

## Page View Counter

```
INCR page_views
```

## Rate Limiting

```
INCR api_user_1
EXPIRE api_user_1 60
```

---

# Best Practices

* Use **SET with EX** instead of SETEX when possible
* Keep values small for better performance
* Use **MGET/MSET** to reduce network overhead

---

# Useful Redis CLI Example

```
redis-cli
SET user:name "John"
GET user:name
INCR visits
```

---

# Conclusion

Redis strings are extremely versatile and form the backbone of many Redis-based systems such as:

* Caching
* Session storage
* Counters
* Rate limiting
* Distributed locks

Understanding string commands is essential for **backend, DevOps, and MLOps engineers** using Redis.
