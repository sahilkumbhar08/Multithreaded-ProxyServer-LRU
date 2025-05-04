
# **Multithreaded Proxy Server with LRU Cache and Rate Limiting**

## Overview

This is a Java-based multithreaded proxy server designed to efficiently handle HTTP GET requests. It incorporates key features like LRU caching for fast repeated requests, rate limiting to prevent excessive traffic, and cache statistics tracking for performance monitoring.

### Features

#### 1️⃣ **Multithreaded Request Handling**
- Uses `ExecutorService` to process multiple client connections concurrently, supporting **GET requests**.
- **Non-blocking I/O** for efficient handling under high load (optional for improved scalability).

#### 2️⃣ **LRU Caching for Faster Responses**
- Implements an **LRU (Least Recently Used)** cache to store frequently accessed responses.
- Reduces repeated network calls by serving cached content directly.

#### 3️⃣ **Cache Statistics Tracking**
- Tracks and reports:
  - **Cache Hits** (Requests served from the cache)
  - **Cache Misses** (Requests fetched from the origin server)
  - **Cache Evictions** (Entries removed due to capacity limits)
  - **Total Requests** and **Rate-Limited Requests**.
  
#### 4️⃣ **Rate Limiting with Token Bucket Algorithm**
- Limits the number of requests a client can make within a specified time window.
- Prevents server overload and ensures fair usage by all clients.
- **Dynamic Rate Limiting** (Customizable based on client needs or IP address).

#### 5️⃣ **Cache Time-to-Live (TTL)**
- Set TTL for cached responses to ensure freshness. Expired cache entries are evicted or fetched from the origin server.

#### 6️⃣ **Cache Preloading**
- Preload frequently requested URLs into the cache during startup for faster initial responses.

#### 7️⃣ **Customizable Error Handling & Caching Rules**
- Allows clients to configure cache eviction policies, rate limits, and other behavior through an API.

#### 8️⃣ **Compression Support**
- Supports caching and serving compressed responses (gzip, Brotli), reducing bandwidth usage.

#### 9️⃣ **Distributed Cache Support (Optional)**
- Integrates with distributed caching systems (e.g., Redis) for scaling out the proxy server across multiple nodes.

#### 🔟 **Security (HTTPS Support)**
- Fully supports encrypted HTTPS connections, ensuring secure communication between the proxy server, clients, and origin servers.

---

## Example Usage

### 1️⃣ **GET Request through Proxy**
**Request:**
```http
GET http://example.com
```

**Result (First Request - Cache Miss):**
```text
Fetching from original server: http://example.com Cache Miss - Storing response in cache.
```

**Result (Subsequent Request - Cache Hit):**
```text
Serving from cache: http://example.com Cache Hit - Response served from cache.
```

---

### 2️⃣ **Rate Limiting in Action**
**Request:**
```http
GET http://example.com (Sent multiple times rapidly)
```

**Result:**
```text
Request blocked - Too many requests from this client.
Request blocked - Too many requests from this client.
Serving from cache: http://example.com
Request blocked - Too many requests from this client.
```

---

### 3️⃣ **Cache Statistics**
**Request:**
```http
GET http://localhost:8080/stats
```

**Result (Example Response):**
```json
{
  "cache_hits": 5,
  "cache_misses": 2,
  "cache_evictions": 1,
  "current_cache_size": "50MB",
  "rate_limit_exceeded": 3,
  "total_requests_handled": 100
}
```

---

## Configuration and API Endpoints

### 1. **/configure-cache**
Allows users to configure cache behavior, TTL, eviction strategy, and preload URLs.

**Request:**
```json
POST /configure-cache
{
  "eviction_policy": "LFU",
  "ttl_seconds": 3600,
  "preload_urls": ["http://example.com", "http://example2.com"]
}
```

**Response:**
```json
{
  "status": "Cache configuration updated successfully."
}
```

---

### 2. **/metrics**
Exposes server metrics for monitoring performance.

**Request:**
```http
GET /metrics
```

**Response:**
```json
{
  "total_requests": 1200,
  "active_connections": 10,
  "rate_limited_requests": 50,
  "cache_hits": 650,
  "cache_misses": 250
}
```

---

## Example Setup

1. **Run the Proxy Server:**
   - Start the proxy server by executing the main Java class.
   - Optionally, configure the server settings using the provided API or `config.json` file.

2. **Start Using the Proxy Server:**
   - Configure your HTTP client (e.g., browser or curl) to use the proxy.
   - Send GET requests to the server and observe cache hits, rate limits, and statistics.

---

## Future Enhancements

- **Asynchronous Request Handling** using Java’s `NIO` or libraries like `Netty` for better performance.
- **Advanced Rate Limiting** strategies, such as adaptive rate limits based on request volume and user behavior.
- **Web Interface** for easier monitoring and configuration of cache statistics and rate limits.

---

Feel free to modify the `config.json` or use the provided APIs to customize the proxy server based on your needs!
