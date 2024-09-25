## Reference Information
- https://www.udemy.com/course/fundamentals-of-backend-communications-and-protocols/learn/lecture/34629842#overview

---
## 5 - TCP


### Use Cases

- Reliable Communication
- Remote shell
- Database connections
- Web communications (h1 and h2 are built on tcp)
- Any bidirectional communication (websockets)

---
### TCP Connection

Connection exists in Layer 5 (session), You must create a connection to send data

Connection is identified by **4** properties

- Source IP:port
- Destination IP:port

These 4 properties are hashed and used to look up if a connection exists, whenever something is received by the server.

---
Data can't be sent outside a connection, it is sometimes called socket or file descriptor.

Requires a **3-way TCP handshake**

Segments are sequenced and ordered and require acknowledgement, lost segments are transmitted.

---
#### Connection Establishment

Client - SYN -> Server
Client <- SYN/ACK - Server
Client - ACK -> Server

This happens so that client and server agree on the terms of sending the data

---
#### Closing a connection

Uses a **4 Way Handshake**

Client - FIN -> Server
Client <- ACK - Server
Client <- FIN - Server
Client - ACK -> Server

---
### TCP Segment

