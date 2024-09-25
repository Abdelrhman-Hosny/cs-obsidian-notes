## Reference Information
- https://www.udemy.com/course/fundamentals-of-backend-communications-and-protocols/learn/lecture/34629832#content

---
## 1 - Protocols Properties

### What is a protocol ?

A system that allows two parties to communicate depending on **certain properties**

---
### Protocol Properties

- Data Format
	- Text based (plain text, JSON, XML)
	- Binary (protobuf, http2 (h2), h3)
- Transfer mode
	- Message based (UDP, HTTP)
	- Stream (TCP, WebRTC)
		- The client has to parse the requests to figure out where the start and end of each request is, this can be costly
- Addressing system
	- DNS name, IP, MAC
- State
	- Stateful (TCP, gRPC)
	- Stateless (UDP, HTTP)
- Routing
	- Proxies, Gateways
- Flow & Congestion Control
	- TCP (Flow & Congestion)
	- UDP (No Control)
- Error Management
	- Timeouts & Retries
	- Error Codes
