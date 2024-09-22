## Reference Information
- https://www.udemy.com/course/fundamentals-of-backend-communications-and-protocols/learn/lecture/34629822#overview

---
## Server Side Events

It is One Request, and a very very long response

SSEs work with HTTP, without needing to setup a websocket like we do in [[Push|Push Model]].

SSEs require atleast HTTP 1.1, but it is preferred to be used with HTTP 2.

HTTP 1.1 has a maximum of 6 connections.

![[Server Side Events 2024-09-01 20.17.37.excalidraw]]

### Pros
- Real time
- Compatible with Request/response
### Cons
- Client must be online
- Client might not be able to handle all the events
- HTTP/1.1 problem (6 connections)