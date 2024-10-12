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

TCP segment is 20 Bytes but can go up to 60 Bytes

It has more headers than the [[4 - UDP#UDP Datagram]] like Segments,Flow Control and Sequence Numbers.



![[download (1).png]]

**Sequence** **Numbers** are used to synchronize so that TCP can make sure messages are sent in order.

**Recognition** **number** is also known as ACK number, and is used to confirm that a particular sequence is received. If the **ACK** **flag** is set to 0, this field is ignored.

You can ack and at the same time be sending data at the same time.

**Reception Window Size** used for flow control. Each client sends to the other the maximum window size it can handle in MBs, it's max is 65kb but can go up to 1 gb. If certain flags are set.

There are 9 flag bits.
- SYN, used to start connection 
- ACK, explained above
- FIN, ends the connection 
- PSH, indicates data is pushed
- RST, restarts the connection from the beginning 
- URG
- ECE
   - used for congestion control
- CWR
   - short for Congestion Window Reduced, to inform the sender that It knows there was about to be a congestion and has already reduced the window size.
---
#### Max Segment Size

Segment Size depends on the Maximum Transmission Unit (MTU) which is a property of the network.


Default MTU is 1500 -> MSS is 1460.

MTUs can be larger in Jumbo frames, which are special servers that allow that.

---



