# Redis as a Cache

## Introduction

Redis is an in-memory key-value database widely used for caching. Because it stores data in RAM instead of disk, it can return data extremely fast (microseconds). This makes Redis ideal for reducing database load and speeding up applications.

---

## What is Caching?

Caching means storing frequently accessed data in a temporary storage layer so that future requests for that data can be served faster.

Without cache:
User -> Application -> Database -> Application -> User

With cache:
User -> Application -> Cache (Redis) -> Application -> User

If the data is not found in the cache, the application queries the database and stores the result in Redis for future use.

---

## Why Redis is Used for Caching

| Feature             | Explanation                                     |
| ------------------- | ----------------------------------------------- |
| In-memory storage   | Data is stored in RAM, making it extremely fast |
| Key-value structure | Easy and fast lookup                            |
| Expiration support  | Keys can expire automatically                   |
| High throughput     | Can handle millions of operations per second    |
| Simple data types   | Strings, lists, sets, hashes, sorted sets       |

---

## Basic Redis Cache Workflow

1. User requests data
2. Application checks Redis cache
3. If data exists -> return data (cache hit)
4. If data does not exist -> fetch from database (cache miss)
5. Store data in Redis
6. Return data to user

---

## Example Scenario

Suppose you have an e-commerce website and users frequently view product details.

Database query might take: 200 ms
Redis lookup might take: 1 ms

### Without Redis

Every request hits the database.

### With Redis

Product data is cached and returned instantly.

---

## Example: Redis Cache with Python

### Install Redis client

```bash
pip install redis
```

### Python Example

```python
import redis

cache = redis.Redis(host='localhost', port=6379, db=0)

product_id = "product:101"

# Check cache
product = cache.get(product_id)

if product:
    print("Cache hit")
    print(product)
else:
    print("Cache miss")

    # simulate database call
    product = "Laptop - $1200"

    cache.set(product_id, product)

    print(product)
```

---

## Cache Expiration (TTL)

Sometimes cached data should expire automatically.

Example:

```python
cache.set("product:101", "Laptop - $1200", ex=60)
```

This means the cache will expire after 60 seconds.

---

## Cache Hit vs Cache Miss

| Term       | Meaning                 |
| ---------- | ----------------------- |
| Cache Hit  | Data found in cache     |
| Cache Miss | Data not found in cache |

High cache hit ratio = faster application.

---

## Cache Invalidation

When database data changes, cache must be updated or removed.

Example:

```python
cache.delete("product:101")
```

---

## Common Redis Cache Patterns

### Cache Aside (Lazy Loading)

Application manages the cache.

Steps:

1. Check cache
2. If missing -> query DB
3. Store in cache

Advantages:

* Simple
* Flexible

---

### Write Through Cache

Data written to cache and database simultaneously.

Flow:
Application -> Cache -> Database

Advantages:

* Cache always consistent

---

### Write Back Cache

Data written to cache first.
Database updated later asynchronously.

Advantages:

* Very fast writes

Disadvantages:

* Risk of data loss

---

### Read Through Cache

Cache automatically loads data from database.
Application only interacts with cache.

---

## Where Redis Cache is Used

| Use Case             | Description                    |
| -------------------- | ------------------------------ |
| Session storage      | Store user login sessions      |
| API response caching | Cache expensive API calls      |
| Rate limiting        | Track API usage                |
| ML feature store     | Store frequently used features |
| Web page caching     | Cache rendered pages           |

---

## Redis Cache in ML / Data Science Projects

Example use cases:

### Feature Store Cache

ML models often reuse the same features.
Redis stores precomputed features.

### Model Prediction Cache

If a model receives the same input multiple times, store prediction in Redis.

---

## Example: ML Prediction Cache

```python
input_data = "user123_features"

prediction = cache.get(input_data)

if prediction:
    print("Prediction from cache")

else:
    prediction = model.predict(features)
    cache.set(input_data, prediction)
```

---

## Advantages of Using Redis as Cache

| Advantage                | Explanation                    |
| ------------------------ | ------------------------------ |
| Extremely fast           | In-memory storage              |
| Reduces DB load          | Many queries served from cache |
| Scalable                 | Supports clustering            |
| Flexible data structures | Multiple data types            |

---

## Limitations

| Limitation                    | Explanation             |
| ----------------------------- | ----------------------- |
| RAM cost                      | Memory is expensive     |
| Data loss risk                | If persistence disabled |
| Cache invalidation complexity | Must manage updates     |

---

## Best Practices

1. Use TTL for cached data
2. Avoid caching very large objects
3. Monitor cache hit ratio
4. Use Redis clusters for scalability
5. Use proper key naming

Example key naming:

```
user:1001:profile
product:245:details
model:prediction:user123
```

---

## Redis Cache Architecture Example

```
User
  |
  v
Application Server
  |
  |-----> Redis Cache
  |
  v
Primary Database
```

Flow:

1. Application checks Redis
2. If data exists -> return
3. If not -> query DB
4. Store result in Redis

---

## Summary

Redis is one of the fastest caching systems available today. It significantly improves application performance by reducing database load and serving frequently accessed data from memory.

Because of its speed, scalability, and flexibility, Redis is widely used in modern systems including web applications, ML pipelines, and real-time analytics.
