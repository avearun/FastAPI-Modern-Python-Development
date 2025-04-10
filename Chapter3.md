Chapter3.md

# Chapter 3 FastAPI Tour
# pg 30

Creating first api 

If you use **`httpx`** instead of **`requests`**, the code would work very similarly, but there are a few key differences in behavior and features. Here's how the equivalent code would look and what changes you might expect:

### **1. Basic Equivalent Code**
```python
import httpx

# Make a GET request
r = httpx.get("http://localhost:8000/hi")

# Parse JSON response
print(r.json())  # Output: 'Hello? World?'
```
Just like `requests`, `httpx` provides:
- `.get()`, `.post()`, etc., for HTTP methods.
- `.json()` to parse JSON responses.

---

### **2. Key Differences Between `httpx` and `requests`**
#### **(A) Async Support (Big Advantage of `httpx`)**
`httpx` supports **both synchronous and asynchronous** requests, whereas `requests` is only synchronous.

**Async Example:**
```python
import httpx
import asyncio

async def fetch_data():
    async with httpx.AsyncClient() as client:
        r = await client.get("http://localhost:8000/hi")
        return r.json()

# Run the async function
print(asyncio.run(fetch_data()))  # Output: 'Hello? World?'
```

#### **(B) HTTP/2 Support**
- `httpx` supports **HTTP/2** (faster in some cases), while `requests` only supports HTTP/1.1.
- You can enable it with:
  ```python
  client = httpx.Client(http2=True)
  ```

#### **(C) Timeout Handling**
- `httpx` has stricter timeout handling by default.
- Example:
  ```python
  httpx.get("http://localhost:8000/hi", timeout=10.0)  # 10-second timeout
  ```

#### **(D) Streaming Responses**
- `httpx` has a more flexible streaming API:
  ```python
  with httpx.stream("GET", "http://localhost:8000/hi") as r:
      for chunk in r.iter_bytes():
          print(chunk)
  ```

#### **(E) Automatic JSON Handling**
- Both libraries parse JSON similarly, but `httpx` raises `httpx.JSONDecodeError` instead of `requests.JSONDecodeError` on invalid JSON.

---



