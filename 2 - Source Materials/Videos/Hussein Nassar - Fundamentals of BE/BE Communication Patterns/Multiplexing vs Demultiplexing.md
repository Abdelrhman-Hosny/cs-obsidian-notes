## Reference Information
- https://www.udemy.com/course/fundamentals-of-backend-communications-and-protocols/learn/lecture/34629826#content

---
## Multiplexing vs Demultiplexing

![[Multiplexing vs Demultiplexing 2024-09-02 14.38.38.excalidraw]]

---
### **Multiplexing** 
refers to the technique of combining multiple signals, messages, or data streams into a single channel or connection. In backend systems, it allows multiple independent communication flows to share a single physical or logical connection.

#### **How It Works**:

- In a backend system, multiplexing allows multiple services or processes to communicate over a single network connection or channel. This is particularly useful when dealing with high-performance systems where managing separate connections for each communication flow would be resource-intensive.
- The multiplexed data is typically tagged or labeled with identifiers that help the receiving end (demultiplexer) separate and route the data to the correct destination.

#### **Examples**:

- **HTTP/2 Multiplexing**: In HTTP/2, multiple streams of data (requests and responses) can be sent over a single TCP connection. Each stream is identified by a unique stream ID, allowing the client and server to handle multiple concurrent requests/responses without waiting for one to complete before starting another.
- **WebSocket Multiplexing**: Multiple logical channels of communication can be established over a single WebSocket connection, enabling different types of messages or interactions to be handled concurrently.
- **Message Queues**: When multiple producers publish messages to a single message queue, this can be seen as a form of multiplexing. The queue aggregates messages from various sources into one place for processing.

---
### **Demultiplexing**

 is the counterpart to multiplexing. It refers to the process of taking a single input stream that contains multiple signals or data flows and separating them out into individual, identifiable streams for processing or routing to their respective destinations.

#### **How It Works**:

- After data is multiplexed and sent through a single channel, the receiving system must demultiplex it. This involves reading the identifiers or tags associated with each part of the multiplexed data, then routing or processing each part according to its identifier.
- Demultiplexing is crucial for ensuring that the right data reaches the right service or process, especially in systems where a single connection is used to handle multiple types of communication.

#### **Examples**:

- **Operating System Networking Stack**: The OS uses demultiplexing to direct incoming network packets to the correct application or process based on information in the packet headers (e.g., port numbers in TCP/UDP packets).

- **Microservices**: A service might receive a multiplexed data stream from an API gateway, which it then demultiplexes to route requests to the appropriate microservice or handler.

- **Message Brokers**: When a message broker like Kafka or RabbitMQ receives a multiplexed stream of messages, it demultiplexes them based on topics, queues, or routing keys, ensuring each message reaches its intended consumer.
