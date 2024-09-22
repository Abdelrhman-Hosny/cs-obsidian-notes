
## Difference between `HttpRouter` & `HttpHandler`

### Handler

Used to handle http requests that match a specific route (exact match)

if route is matched, handler calls the registered function

### Router

A more feature rich component built on top of handlers.

routes the http request to the appropriate handler.

features allow matching of dynamic routing, path variables, middleware and route grouping.

---

## `HttpServer` vs `Web Server`

`HttpServer` is used by backend frameworks to serve http requests

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
def read_item(item_id: int, q: str = None):
    return {"item_id": item_id, "query": q}

# Start the FastAPI server using Uvicorn
if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="127.0.0.1", port=8000)

```

This is good for development and testing, or for microservices

`Web Server` examples are nginx, Apache webserver

They are full-featured software designed to serve websites, and handle large-scale traffic and provide things like security, load balancing, caching and more

They are used in production environments where reliability, scalability, and performance are critical.

They are used to handle everything from static content delivery to complex dynamic content and API routing.

---
The `sql.Open()` function doesn’t actually create any connections, all it does is initialize the
pool for future use. Actual connections to the database are established lazily, as and when
needed for the first time. So to verify that everything is set up correctly we need to use the
`db.Ping()` method to create a connection and check for any errors.

---
