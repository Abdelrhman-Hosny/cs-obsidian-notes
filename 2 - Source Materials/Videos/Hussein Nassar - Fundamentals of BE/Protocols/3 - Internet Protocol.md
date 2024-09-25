## Reference Information
- https://www.udemy.com/course/fundamentals-of-backend-communications-and-protocols/learn/lecture/34648342#content

---
## 3 - Internet Protocol

Contains information that are already covered in Uni, things like Gateways and how a router distributes an incoming request, and how does it know that a request is in the same local network or not using subnets and subnet masks.

Then the IP packet is discussed and explained.

---
### ICMP

A Layer 3 Protocol.
short for Internet Control Message Protocol.

Designed for informational messages

- Host unreachable, port unreachable, fragmentation needed
- Packet expired (TTL ended)

Used by `ping` and `traceroute`
Doesn't require listeners or ports to be opened

---

- Some firewalls block ICMP for security reasons.
- Disabling ICMP can also cause real damage with connection establishment
	- Fragmentation needed.

