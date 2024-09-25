## Reference Information

---
## 2 - OSI Model

Important if you'll work on anything related to networks, most important thing is to **understand at which layer does your application live.**

---
### Why do we need a communication model ?

- Abstraction
		Imagine if you had to write different versions of code for WiFi vs Ethernet vs LTE ?
- Network Equipment Management
		Upgrading equipment becomes difficult as everybody has his own way to deal with things
- Decoupled Innovation
		You can upgrade Layer 3 Without any effect on Apps working with Layer 4

---
### The OSI Model

#### 7 Layers of OSI

Don't take everything literally, think about this as a way to help you understand. As sometimes, it is hard to determine what each layer exactly does, but you understand the concept as a whole, which is the important bit.

##### L7 Application Layer

This is the layer that the most backend engineers work on, for a **network engineer** all he really cares about in that layer is maybe what communication protocol are you using.

All the code and complex logic doesn't affect a network engineer, just that you use **http** or **gRPC**.

##### L6 Presentation

It is the serialization and encoding of data to be in byte format so it can be transmitted to the network

P.S. People often argue that this doesn't require a complete layer.

##### L5 Session

This is what stores the state of the session, it does not exist in some protocols like UDP.

But in something like TCP, where you need to establish a connection it does exist.

Some apps are totally Layer 5 apps, like proxies that just does some small logic totally unrelated to what your app does.

##### L4 Transport

We are aware of the UDP or TCP. and what type of data is transferred inside them.

##### L3 Network

Sends Packets by IP, doesn't know about ports

##### L2 Data Link

Sends Frames over MAC Address

##### L1 Physical

---
#### Examples By Sender

A sender is sending a post request

L7 -> POST request with Json data to HTTPS server
L6 -> Serialize the JSON
L5 -> Request to establish TCP connection/TLS
L4 -> Sends a SYN request target port 443
L3 -> SYN is placed in packets and adds source/dest IPs
L2 -> Each packet goes into a single frame and adds src/dest MAC address.
L1 -> turned into electric signals ..etc

---
### Shortcomings of the OSI Model

- Has a lot of layers which makes it hard to comprehend
- Hard to argue what each layer does
- Simpler to deal with layers (5-6-7) as a single layer
	- This happens in TCP/IP Model

---
### TCP/IP Model

Simpler than the OSI Model with just **4** layers

Application -> Layers 5, 6, 7 in OSI
Transport -> Layer 4 in OSI
Internet -> Layer 3 in OSI
Data Link -> Layer 2 in OSI

No mention of Physical Layer


