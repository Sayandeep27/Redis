# Redis Key Management Commands – Complete Guide

## Overview

Redis **Key Management Commands** are used to manage keys in the database regardless of the data type stored inside them.

These commands allow you to:

* Create and delete keys
* Check key existence
* Set expiration (TTL)
* Move or copy keys between databases
* Scan and iterate through keys
* Inspect metadata about keys

Key management commands are fundamental for **database maintenance, caching strategies, and memory management**.

---

# Basic Key Commands

| Command | Description                     | Example         |
| ------- | ------------------------------- | --------------- |
| DEL     | Delete one or more keys         | `DEL user:1`    |
| UNLINK  | Asynchronously delete keys      | `UNLINK user:1` |
| EXISTS  | Check if key exists             | `EXISTS user:1` |
| TYPE    | Get type of value stored in key | `TYPE user:1`   |

---

# Expiration (TTL) Commands

Redis allows keys to automatically expire after a specified time.

| Command     | Description                      | Example                             |
| ----------- | -------------------------------- | ----------------------------------- |
| EXPIRE      | Set expiration time in seconds   | `EXPIRE session:1 60`               |
| PEXPIRE     | Set expiration in milliseconds   | `PEXPIRE session:1 1000`            |
| EXPIREAT    | Set expiration at UNIX timestamp | `EXPIREAT session:1 1700000000`     |
| PEXPIREAT   | Expire at UNIX time (ms)         | `PEXPIREAT session:1 1700000000000` |
| TTL         | Remaining time to live (seconds) | `TTL session:1`                     |
| PTTL        | Remaining TTL in milliseconds    | `PTTL session:1`                    |
| EXPIRETIME  | Get expiration timestamp         | `EXPIRETIME session:1`              |
| PEXPIRETIME | Get expiration timestamp (ms)    | `PEXPIRETIME session:1`             |
| PERSIST     | Remove expiration from key       | `PERSIST session:1`                 |

---

# Key Renaming Commands

| Command  | Description                      | Example              |
| -------- | -------------------------------- | -------------------- |
| RENAME   | Rename key                       | `RENAME key1 key2`   |
| RENAMENX | Rename if new key does not exist | `RENAMENX key1 key2` |

---

# Key Scanning & Iteration

| Command | Description                | Example       |
| ------- | -------------------------- | ------------- |
| KEYS    | Find keys matching pattern | `KEYS user:*` |
| SCAN    | Incrementally iterate keys | `SCAN 0`      |

---

# Key Copying & Migration

| Command | Description                      | Example                            |
| ------- | -------------------------------- | ---------------------------------- |
| COPY    | Copy key to another key          | `COPY key1 key2`                   |
| MOVE    | Move key to another database     | `MOVE key1 1`                      |
| MIGRATE | Move key to another Redis server | `MIGRATE host port key db timeout` |

---

# Serialization Commands

| Command | Description            | Example                           |
| ------- | ---------------------- | --------------------------------- |
| DUMP    | Serialize key value    | `DUMP key1`                       |
| RESTORE | Restore serialized key | `RESTORE key2 0 serialized-value` |

---

# Random Key Access

| Command   | Description       | Example     |
| --------- | ----------------- | ----------- |
| RANDOMKEY | Return random key | `RANDOMKEY` |

---

# Key Metadata Commands

| Command         | Description              | Example                |
| --------------- | ------------------------ | ---------------------- |
| OBJECT ENCODING | Encoding used internally | `OBJECT ENCODING key1` |
| OBJECT FREQ     | Access frequency counter | `OBJECT FREQ key1`     |
| OBJECT IDLETIME | Idle time of key         | `OBJECT IDLETIME key1` |
| OBJECT REFCOUNT | Reference count          | `OBJECT REFCOUNT key1` |

---

# Touch Command

| Command | Description             | Example           |
| ------- | ----------------------- | ----------------- |
| TOUCH   | Update last access time | `TOUCH key1 key2` |

---

# Complete Redis Key Management Command Reference

| Command         |
| --------------- |
| COPY            |
| DEL             |
| DUMP            |
| EXISTS          |
| EXPIRE          |
| EXPIREAT        |
| EXPIRETIME      |
| KEYS            |
| MIGRATE         |
| MOVE            |
| OBJECT ENCODING |
| OBJECT FREQ     |
| OBJECT IDLETIME |
| OBJECT REFCOUNT |
| PERSIST         |
| PEXPIRE         |
| PEXPIREAT       |
| PEXPIRETIME     |
| PTTL            |
| RANDOMKEY       |
| RENAME          |
| RENAMENX        |
| RESTORE         |
| SCAN            |
| TOUCH           |
| TTL             |
| TYPE            |
| UNLINK          |

---

# Redis CLI Example

```
redis-cli

SET user:1 "Alice"
EXPIRE user:1 60
TTL user:1
TYPE user:1
DEL user:1
```

---

# Best Practices

* Avoid using **KEYS** in production; prefer **SCAN**.
* Use **UNLINK** instead of DEL for large datasets.
* Always set **TTL for cache keys**.
* Use **MIGRATE** for moving data between Redis instances.

---

# Conclusion

Redis key management commands are essential for controlling the lifecycle of data inside Redis.

They enable:

* Efficient cache management
* Expiration control
* Data migration
* Database maintenance

Understanding these commands is crucial for **backend developers, DevOps engineers, and MLOps professionals working with Redis infrastructure**.
