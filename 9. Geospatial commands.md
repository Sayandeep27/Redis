# Redis Geospatial Commands – Complete Guide

## Overview

Redis provides **Geospatial (Geo) commands** to store, index, and query geographic location data (longitude and latitude).

Internally, Redis stores geospatial data inside **Sorted Sets (ZSETs)** using a special encoding called **Geohash**.

This allows Redis to perform **very fast radius queries and distance calculations**.

Typical use cases include:

* Ride sharing apps (Uber, Ola)
* Food delivery services
* Nearby store lookup
* Location-based recommendations
* Real-time location tracking

---

# How Redis Stores Geospatial Data

Redis uses the following format:

```
GEOADD key longitude latitude member
```

Example:

```
GEOADD cities 77.1025 28.7041 "Delhi"
GEOADD cities 72.8777 19.0760 "Mumbai"
```

Under the hood:

* Data is stored in a **Sorted Set**
* The score represents a **Geohash encoding of the coordinates**

---

# Core Geospatial Commands

| Command | Description                   | Example                           |
| ------- | ----------------------------- | --------------------------------- |
| GEOADD  | Add location data             | `GEOADD cities 77.10 28.70 Delhi` |
| GEOPOS  | Get coordinates of members    | `GEOPOS cities Delhi`             |
| GEODIST | Distance between two members  | `GEODIST cities Delhi Mumbai km`  |
| GEOHASH | Get geohash string of members | `GEOHASH cities Delhi`            |

---

# Radius Queries (Deprecated but Still Supported)

These commands search for members within a given radius.

| Command           | Description                        | Example                                 |
| ----------------- | ---------------------------------- | --------------------------------------- |
| GEORADIUS         | Find members within radius         | `GEORADIUS cities 77 28 500 km`         |
| GEORADIUSBYMEMBER | Radius query using member location | `GEORADIUSBYMEMBER cities Delhi 500 km` |

Note:

These commands are **deprecated in Redis 6.2+** and replaced by **GEOSEARCH**.

---

# Modern Geo Query Commands (Recommended)

Redis introduced new commands that replace the older radius queries.

| Command        | Description                                | Example                                                         |
| -------------- | ------------------------------------------ | --------------------------------------------------------------- |
| GEOSEARCH      | Search locations by radius or bounding box | `GEOSEARCH cities FROMLONLAT 77 28 BYRADIUS 500 km`             |
| GEOSEARCHSTORE | Store results of GEOSEARCH in another key  | `GEOSEARCHSTORE nearby cities FROMLONLAT 77 28 BYRADIUS 500 km` |

---

# Units Supported in Distance Queries

| Unit | Description |
| ---- | ----------- |
| m    | Meters      |
| km   | Kilometers  |
| mi   | Miles       |
| ft   | Feet        |

Example:

```
GEODIST cities Delhi Mumbai km
```

---

# Common Query Options

Redis geo queries support additional options.

| Option    | Description                 |
| --------- | --------------------------- |
| WITHDIST  | Return distance from center |
| WITHCOORD | Return coordinates          |
| WITHHASH  | Return geohash              |
| COUNT     | Limit number of results     |
| ASC       | Sort ascending by distance  |
| DESC      | Sort descending by distance |

Example:

```
GEOSEARCH cities
FROMLONLAT 77 28
BYRADIUS 500 km
WITHDIST
WITHCOORD
ASC
```

---

# Complete Redis Geospatial Command Reference

| Command           |
| ----------------- |
| GEOADD            |
| GEODIST           |
| GEOHASH           |
| GEOPOS            |
| GEORADIUS         |
| GEORADIUSBYMEMBER |
| GEOSEARCH         |
| GEOSEARCHSTORE    |

---

# Real World Examples

## Find Nearby Restaurants

```
GEOADD restaurants 77.2090 28.6139 "Restaurant1"
GEOADD restaurants 77.1025 28.7041 "Restaurant2"

GEOSEARCH restaurants
FROMLONLAT 77.20 28.61
BYRADIUS 10 km
```

---

## Distance Between Two Cities

```
GEODIST cities Delhi Mumbai km
```

---

## Find Nearby Drivers (Ride Sharing)

```
GEOSEARCH drivers
FROMLONLAT 77.20 28.61
BYRADIUS 5 km
WITHDIST
COUNT 5
ASC
```

---

# Redis CLI Example

```
redis-cli

GEOADD cities 77.1025 28.7041 Delhi
GEOADD cities 72.8777 19.0760 Mumbai

GEOPOS cities Delhi
GEODIST cities Delhi Mumbai km

GEOSEARCH cities FROMLONLAT 77 28 BYRADIUS 1000 km
```

---

# Best Practices

* Prefer **GEOSEARCH over GEORADIUS** (modern Redis).
* Use **COUNT** to limit large result sets.
* Use **WITHDIST** for distance-based sorting.
* Combine geospatial queries with **Sorted Set operations** for advanced analytics.

---

# Performance Characteristics

| Operation | Complexity   |
| --------- | ------------ |
| GEOADD    | O(log N)     |
| GEODIST   | O(1)         |
| GEOSEARCH | O(N + log M) |
| GEOPOS    | O(log N)     |

---

# Conclusion

Redis geospatial commands allow developers to efficiently handle **location-based data at scale**.

They enable:

* Fast geographic indexing
* Radius and proximity searches
* Distance calculations
* Real-time location services

These features make Redis ideal for building **location-aware applications such as ride sharing, logistics tracking, delivery systems, and mapping services**.
