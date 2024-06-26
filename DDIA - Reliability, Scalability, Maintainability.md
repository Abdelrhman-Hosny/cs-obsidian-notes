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

Once you describe the load on the system.

- When you increase a load parameter and keep the system resources unchanged, how is the performance of your system affected?
- when you increase a load parameter, how much resources do you need to increase to keep performance ?

It i
#### Latency and response time

Response time: What the client sees.
Latency: Duration that a request is waiting to be handled



## Maintainability 



