## Reference Information
- https://www.udemy.com/course/fundamentals-of-backend-communications-and-protocols/learn/lecture/34629816#questions

---
## Short Polling
![[Short Polling 2024-09-01 17.30.46.excalidraw]]

### Pros

- Simple
- Good for long running requests
- Client can disconnect and ask for `requestId` later on
	- However, request result might expire if client disconnects for too long

### Cons

- Too chatty
	- Imagine if the amount of users scale too much, this would create an overhead as you would send too many requests to the backend (most of which are empty)
- Network Bandwidth
	- Cloud providers bills would be too high because of too many requests
- Wasted backend resources