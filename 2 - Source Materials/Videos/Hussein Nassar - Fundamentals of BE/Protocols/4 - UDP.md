## Reference Information

- https://www.udemy.com/course/fundamentals-of-backend-communications-and-protocols/learn/lecture/34629838#questions

---
## 4 - UDP

User Datagram Protocol is a **Stateless** Layer **4** protocol.

No need to establish a connection in order to start sending a message.

Communicates with **processes** in a host using **ports**

Prior communication is not required (double edged sword)

---
### Use cases

Usually used in cases where you don't need to reliably receive all the data.
- Video streaming
- VPN
- DNS
- WebRTC

---
### UDP Datagram

UDP Header is only 8 bytes, and is considered as "data" in the IP packet.

Port is a 16-bit int (0 -> 65535).

Contains
- Src/dest ports
- Data
- Length of Data
- Checksum

---
### UDP Pros and Cons

#### UDP Pros

- Simple
- Small header size
- Stateless
- Consumes less memory
- Low Latency - no handshakes, order, re-transmission or guaranteed delivery
#### UDP Cons

- No Ack -> No guaranteed delivery
- Connectionless - Anyone can send data without prior knowledge
	- You can send a lot of packets to one server, and the server will have to process them, and a lot of requests can cause a DoS (Denial of Service)
	- Sometimes, attackers change the source IP and send datagrams to DNS Servers, the DNS servers flood the changed source IP with a response, causing a failure (DNS Flooding)
- No flow control 
	- No clue if server/client can handle the data you are sending
	- You could build that at the application level, then you'd be a layer 4 app
- No congestion control
- No ordered packets
- Security - can be easily spoofed
	- Spoofing means somebody pretending to be someone else.
---




