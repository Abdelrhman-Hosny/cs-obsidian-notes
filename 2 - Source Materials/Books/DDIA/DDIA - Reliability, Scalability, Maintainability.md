## Reference Information
- Kleppmann, Martin. "Chapter 1" in _Designing Data-Intensive Applications_, O'Reilly Media, 2017.

---
# DDIA - Reliability, Scalability, Maintainability

Apps created nowadays build a data system using different general purpose tools.

During building apps, we go focus on three main concerns

## Reliability 

When we say an app is reliable people typically expect:
- app performs functions expected by user
- app can tolerate the user making mistakes
- good performance under **expected load and data volume**
- prevent user abuse

Things that go wrong are called **faults**
### Fault vs Failure
Fault means one component does something unexpected.
Failure is the system as a whole stops working

Fault-tolerant systems prevents faults from causing failures.

### Hardware Faults

With the increase in the number of nodes in a system, hardware failure in one of these nodes is more common.

Hardware faults are thought of as being random and independent.
### Software Faults

Highly dependent on the application as they are usually a result of a bug in the code.

### How important is Reliability?

Clients dislike unreliable apps even if the app is non critical.

To be practical, you have to know where and when you are cutting corners.
## Scalability
One common factor that causes systems to degrade is increase of users.
E.g. 10k -> 100k users

No system is scalable for all users, a system is scalable given certain resources and for a certain volume of users.

Diacussing Scalability means considering questions like "How can we add computing resources to handle additional load" ?

The first step is **Describing the load**

#### Describing the load

Load can be described by a few numbers called **load parameters**.

These choices depend on the architecture of the system.

So you consider an operation and describe the parameters in the system.

Operation: # of concurrent documents processed
Parameters: # of nodes in microservice 1, # of workers in microservice 1, # of items in database.

#### Describing Performance

Once you describe the load on the system, you start asking specific question depending on the operations that you need and the resources that you can change.

- When you increase a load parameter and keep the system resources unchanged, how is the performance of your system affected?
- when you increase a load parameter, how much resources do you need to increase to keep performance ?
#### Latency and response time

Response time: What the client sees.
Latency: Duration that a request is waiting to be handled

#### Percentiles and Describing Performance

To judge the latency, it is preferred to use percentiles.

![[percentiles.png]]

Mean response time doesn't tell you how many users experienced that delay.

High percentiles of response times aka **tail latencies** are important as people with the slowest requests might be people who use the app the most (so have more data and requests)

Percentiles are often used in *service level objectives (SLOs)* and *service level agreements (SLAs)*

**head of line blocking** occurs when the query that accepts requests is blocked.
This is often a mistake in testing as tests wait for the previous request to finish before starting the next one, which overlooks any problems related to the queue,

##### Tail latency amplification

Occurs when small percentage of calls are slow, which block the app and causes other requests to also be slower.
## Maintainability 



