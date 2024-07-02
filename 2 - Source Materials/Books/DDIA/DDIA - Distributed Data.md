[[DDIA - Foundations Of Data Systems|Part 1]] of the book discussed a few concepts about data systems, all of those were on a **single machine**

[[DDIA - Distributed Data]] discusses what would happen if **multiple machines** are involved in storage and retrieval

---
## Why have a Distributed DB ?

Scalability, Latency, Availability/Fault-tolerance

---
### Shared Memory vs Shared Disk vs Shared Nothing

#### Shared Memory (Vertical Scaling)
The same machine is upgraded by giving it more cores (i.e. using better processors)
#### Shared Disk

Uses several machines with independent CPUs and RAM, but stores data on an array of disks that is shared between the machines, connected with via a very fast network.

Limited Scalability due to contention and locking.
Limited to 1 geographical location

#### Shared-Nothing Architectures

aka Horizontal Scaling

In this approach each machine or VM running is called a **node** and has its independent RAM, CPU, and disk.

Coordination between nodes id done at the **software level** using a conventional network.

No special hardware is required by a shared-nothing system.

The book focuses on Shared-Nothing Architectures as they require the most caution from the developer.

---
### Replication vs Partitioning

These are the two common ways data is distributed across multiple nodes.


| Replication                                                                                  | Partitioning                                                                                                             |
| -------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Keeps a copy of the same data on several different nodes, potentially in different locations | Splits a database into smaller subsets called partitions so that different partitions can be assigned to different nodes |
| If one node fails, the other can do exactly the same work                                    |                                                                                                                          |

![[partition-replica.png]]

