## Reference Information
- https://www.udemy.com/course/fundamentals-of-backend-communications-and-protocols/learn/lecture/34629344#overview

---
## Request Response Model

### Steps of Request Response Model
- Client Sends Request
- Server Parses Request
- Server Processes the request
- Server sends a Response
- Client Parses the Response and consumes it
---
### Used in
- Web, HTTP, DNS, SSH
- RPC (Remote Procedure Call)
- SQL and Database Protocols
- APIs

---
### Doesn't work everywhere
- Notification Service
- Chatting application
Both of those would require constant polling (i.e. sending requests asking if there is a notification each 5 seconds).

This would spam the network with empty messages.

- Very Long Requests

Better use Async requests, as client might disconnect which would require starting the long request over again.
