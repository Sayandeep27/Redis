# Redis for Rate Limiting

---

## 1. Introduction

Rate limiting is a technique used to control how many requests a user or client can make to a server within a specific time period.

Example:

* Allow **100 API requests per minute per user**.
* If the user exceeds the limit → block the request.

Rate limiting helps prevent:

* API abuse
* DDoS attacks
* Server overload
* Unfair usage

**Redis is commonly used for rate limiting because it is extremely fast and supports atomic operations.**

---

## 2. Why Redis is Good for Rate Limiting

| Feature              | Why It Helps                      |
| -------------------- | --------------------------------- |
| In‑memory database   | Very fast operations              |
| Atomic commands      | Prevents race conditions          |
| TTL (Time To Live)   | Automatically expires counters    |
| Increment operations | Perfect for counting requests     |
| High scalability     | Works well in distributed systems |

---

## 3. Basic Idea of Rate Limiting

The basic idea:

1. Every user has a **counter**.
2. When the user sends a request → increase the counter.
3. If the counter exceeds the limit → reject request.
4. After the time window expires → counter resets.

Example:

Limit = **5 requests per minute**

| Request | Counter | Result  |
| ------- | ------- | ------- |
| 1       | 1       | Allowed |
| 2       | 2       | Allowed |
| 3       | 3       | Allowed |
| 4       | 4       | Allowed |
| 5       | 5       | Allowed |
| 6       | 6       | Blocked |

---

## 4. Simple Redis Rate Limiting Example

### Step 1 — User sends request

User ID: `user_101`

### Step 2 — Redis Key

```
rate_limit:user_101
```

### Step 3 — Increase counter

```
INCR rate_limit:user_101
```

Example response:

```
1
```

### Step 4 — Set expiration (60 seconds)

```
EXPIRE rate_limit:user_101 60
```

Now Redis will automatically delete the key after **60 seconds**.

---

## 5. Example Flow

User sends 4 requests.

Redis values:

| Request | Redis Command | Counter |
| ------- | ------------- | ------- |
| 1       | INCR          | 1       |
| 2       | INCR          | 2       |
| 3       | INCR          | 3       |
| 4       | INCR          | 4       |

If the limit is **3 requests per minute**:

Request 4 → Blocked

---

## 6. Python Implementation Example

### Install Redis

```
pip install redis
```

### Python Code

```python
import redis
import time

r = redis.Redis(host='localhost', port=6379, db=0)

LIMIT = 5
WINDOW = 60


def is_allowed(user_id):

    key = f"rate_limit:{user_id}"

    requests = r.incr(key)

    if requests == 1:
        r.expire(key, WINDOW)

    if requests > LIMIT:
        return False

    return True


for i in range(10):

    if is_allowed("user_1"):
        print("Request allowed")
    else:
        print("Rate limit exceeded")

    time.sleep(5)
```

---

## 7. Rate Limiting Algorithms with Redis

Redis supports multiple rate limiting strategies.

| Algorithm      | Description                           |
| -------------- | ------------------------------------- |
| Fixed Window   | Limit requests in a fixed time window |
| Sliding Window | More accurate request tracking        |
| Token Bucket   | Allows bursts of traffic              |
| Leaky Bucket   | Smooths traffic rate                  |

---

## 8. Fixed Window Rate Limiter

Simplest method.

Example:

* Limit: **100 requests per minute**

Redis key:

```
rate_limit:user_1:minute
```

Commands:

```
INCR key
EXPIRE key 60
```

Problem:

A user can send:

```
100 requests at 00:59
100 requests at 01:00
```

Total = **200 requests in 2 seconds**.

---

## 9. Sliding Window Rate Limiter

More accurate method.

Instead of counters, we store **timestamps of requests**.

Redis command used:

```
ZADD
ZREMRANGEBYSCORE
ZCARD
```

Example flow:

1. Add request timestamp
2. Remove timestamps older than window
3. Count remaining timestamps

If count > limit → block request.

---

## 10. Sliding Window Example

Key:

```
rate_limit:user_1
```

Commands:

```
ZADD rate_limit:user_1 timestamp timestamp
ZREMRANGEBYSCORE rate_limit:user_1 0 old_timestamp
ZCARD rate_limit:user_1
```

---

## 11. Token Bucket Algorithm

In this approach:

* Tokens are added at a constant rate.
* Each request consumes a token.

If no tokens remain → request blocked.

Example:

Bucket capacity = **10 tokens**

Token refill rate = **1 token/sec**

---

## 12. Real‑World Example

### API Rate Limiting

Example:

```
GET /api/predict
```

Limit:

```
50 requests/minute per user
```

Redis Key:

```
rate_limit:user_id
```

Used in:

* ML APIs
* Payment APIs
* Authentication systems

---

## 13. Redis Rate Limiting in Distributed Systems

If you run multiple servers:

```
Server 1
Server 2
Server 3
```

All servers use the **same Redis instance**.

This ensures:

* Shared counters
* Consistent rate limits

---

## 14. Example Architecture

```
User
  |
Load Balancer
  |
App Servers
  |
Redis (Rate Limiter)
```

Flow:

1. User sends request
2. Server checks Redis counter
3. If limit exceeded → reject
4. Otherwise → process request

---

## 15. Advantages of Redis Rate Limiting

| Advantage            | Explanation                   |
| -------------------- | ----------------------------- |
| Extremely fast       | In‑memory operations          |
| Scalable             | Works across multiple servers |
| Atomic               | No race conditions            |
| Easy to implement    | Few Redis commands needed     |
| Automatic expiration | Counters reset automatically  |

---

## 16. Common Redis Commands Used

| Command          | Purpose                     |
| ---------------- | --------------------------- |
| INCR             | Increase request counter    |
| EXPIRE           | Set time limit for counter  |
| SETEX            | Set value with expiration   |
| ZADD             | Add timestamp to sorted set |
| ZCARD            | Count requests              |
| ZREMRANGEBYSCORE | Remove old requests         |

---

## 17. Use Cases

Redis rate limiting is widely used in:

* API Gateways
* Authentication systems
* Login protection
* Web scraping prevention
* ML model APIs

---

## 18. Best Practices

| Practice                 | Explanation             |
| ------------------------ | ----------------------- |
| Use unique keys per user | Prevent shared limits   |
| Use expiration           | Avoid memory growth     |
| Monitor Redis memory     | Prevent overload        |
| Use Redis Cluster        | For large scale systems |

---

## 19. Summary

Redis is one of the best tools for implementing **high‑performance rate limiting**.

Because Redis provides:

* atomic counters
* automatic expiration
* very fast operations

it is widely used in **API gateways, ML systems, and large scale applications**.

---

## 20. Quick Cheat Sheet

| Task                     | Redis Command        |
| ------------------------ | -------------------- |
| Increase request counter | INCR key             |
| Set expiry               | EXPIRE key seconds   |
| Sliding window insert    | ZADD key score value |
| Remove old requests      | ZREMRANGEBYSCORE     |
| Count requests           | ZCARD                |

---

**End of Document**
