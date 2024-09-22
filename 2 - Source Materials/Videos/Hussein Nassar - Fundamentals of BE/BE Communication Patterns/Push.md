## Reference Information
- https://www.udemy.com/course/fundamentals-of-backend-communications-and-protocols/learn/lecture/34629814#notes

---
## Push

Used when you want to give data to a client as soon as possible

![[Push 2024-08-31 22.33.55.excalidraw]]

Can be used for Chat System or Notification Systems.
### Pros and Cons

#### Pros
- Real-time
#### Cons
- Clients must be online
- Clients might not be able to handle
	- You have to know if your client can handle the amount of data you are going to push or else they will crash
- Requires a bidirectional protocol
- Polling is preferred for light clients