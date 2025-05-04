# 🧠 Multithreaded Proxy Server with LRU Cache and Rate Limiting

## 📌 Overview

This is a Java-based multithreaded proxy server that efficiently handles HTTP `GET` requests. It includes:

- 🧲 **LRU Caching** to speed up repeated requests  
- 🛡️ **Rate Limiting** using the Token Bucket algorithm  
- 📈 **Cache Statistics Tracking** for performance analysis  

---

## ⚡ Features

### ✅ Multithreaded Request Handling
- Uses `ExecutorService` to handle multiple clients concurrently.
- Each client request is processed independently in a separate thread.
- Supports HTTP `GET` requests.

### ✅ LRU Caching for Faster Responses
- Implements Least Recently Used (LRU) cache strategy.
- Reduces network load by caching frequently accessed pages.
- Uses `LinkedHashMap` or custom logic to maintain cache order.

### ✅ Cache Statistics Tracking
Tracks:
- `cache_hits`: Number of requests served directly from cache  
- `cache_misses`: Number of requests fetched from the original server  
- `cache_evictions`: Number of entries removed due to cache being full  

### ✅ Rate Limiting (Token Bucket Algorithm)
- Prevents abuse and overload by restricting request frequency per client (IP-based).
- Example: allows 5 requests per 10 seconds, then blocks extra requests temporarily.

---

## 🛠 Example Usage with Results

### 1️⃣ GET Request through Proxy

**Request:**
GET http://example.com

markdown
Copy
Edit

**Response (First Request - Cache Miss):**
Fetching from original server: http://example.com
Cache Miss - Storing response in cache.

markdown
Copy
Edit

**Response (Subsequent Request - Cache Hit):**
Serving from cache: http://example.com
Cache Hit - Response served from cache.

yaml
Copy
Edit

---

### 2️⃣ Rate Limiting in Action

**Request:**
GET http://example.com (sent multiple times rapidly)

makefile
Copy
Edit

**Response:**
Serving from cache: http://example.com
Request blocked - Too many requests from this client.
Request blocked - Too many requests from this client.
Serving from cache: http://example.com

yaml
Copy
Edit

---

### 3️⃣ Cache Statistics

**Request:**
GET http://localhost:8080/stats

css
Copy
Edit

**Response:**
```json
{
  "cache_hits": 5,
  "cache_misses": 2,
  "cache_evictions": 1
}
🚀 How to Run
🧰 Prerequisites
Java 8 or higher

🔧 Compile the Code
bash
Copy
Edit
javac ProxyWSRateLimiter.java
▶️ Run the Proxy Server
bash
Copy
Edit
java ProxyWSRateLimiter
By default, it runs on: localhost:8080

📁 Project Structure
bash
Copy
Edit
.
├── ProxyWSRateLimiter.java     # Main proxy server with rate limiter and cache
├── README.md                   # Project documentation
🔍 References
LRU Caching - Wikipedia

Token Bucket Rate Limiting

Java Networking

👨‍💻 Author
Developed as part of a systems programming and networking project to demonstrate performance-aware proxy design.

yaml
Copy
Edit

---

Let me know if you'd like this exported as a downloadable `.md` file!
