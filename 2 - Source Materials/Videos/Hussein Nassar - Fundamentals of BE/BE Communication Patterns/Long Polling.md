## Reference Information
- https://www.udemy.com/course/fundamentals-of-backend-communications-and-protocols/learn/lecture/34629820#overview

---
## Long Polling

Created to avoid the chattiness of [[Short Polling]]

![[Long Polling 2024-09-01 17.48.43.excalidraw]]

### Pros
- Less chatty and backend friendly
- Client can still disconnect

### Cons
- Not real time
	- If a new message arrives between the time of `tn` and `t(n+1)`, it won't be received unless you query for it again
	- However this is not a very huge delay and might be acceptable for some people