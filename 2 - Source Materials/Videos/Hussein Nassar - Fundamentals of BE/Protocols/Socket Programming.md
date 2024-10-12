A socket is an endpoint for sending or receiving data across a network, it acts as a bridge between the transport layer and application layer.

A socket consists of
- IP
- Port 
- Protocol (TCP, or UDP

Socket can be classified into
- Streaming Socket (TCP Socket)
- Datagram Socket

---
## Creating a TCP Server

### Creating a socket
```c
// TCP Socket
int server_fd = socket(AF_INET, SOCK_STREAM, 0);
```

```c
// UDP Socket
int server_fd = socket(AF_INET, SOCK_DGRAM, 0);
```

### Binding the Socket
  
   Binding associates a socket to a specific ip and port number.
   This also tells the os that the socket will send and receive on them when ready.
#### Binding in UDP

```c
int server_fd = socket(AF_INET, SOCK_DGRAM, 0);
struct sockaddr_in address;
address.sin_family = AF_INET;
address.sin_addr.s_addr = INADDR_ANY;
address.sin_port = htons(PORT);

bind(server_fd, (struct sockaddr *)&address, sizeof(address));
```
`INADDR_ANY` accepts requests from all network interfaces, you could use a specific address instead.

Since UDP is connectionless, it doesn't have to listen and is ready to accept data.

---
#### Binding in TCP

```c
int server_fd = socket(AF_INET, SOCK_STREAM, 0);
struct sockaddr_in address;
address.sin_family = AF_INET;
address.sin_addr.s_addr = INADDR_ANY; // Bind to all available interfaces
address.sin_port = htons(PORT);       // Specify port number

bind(server_fd, (struct sockaddr *)&address, sizeof(address));
```

---
### Listening

Listening is used in connection-oriented protocols like TCP.

It marks the socket as passive and ready to accept conn
#### Listening in TCP

```c
listen(server_fd, BACKLOG); // BACKLOG defines the maximum length for the queue of pending connections
```
