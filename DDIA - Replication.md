## Reference Information

---
## DDIA - Replication

## Handling Changes Across Replicas
There are different algorithms to handle changes across replicas 
- Single Leader
- Multi leader
- Leaderless
They can also be synchronous or asynchronous.

---
This doesn't only apply to databases, it could also be applied to **message queues** and **network systems**

---

### Synchronous vs Asynchronous Replication

When using **single leader** approach, being synchronous means you wait for all followers to confirm that the write is made before confirming to the user.


![[Semi synchronousReplication.jpg]]

### Semi Synchronous Replication 
1 follower only is sync and the rest are sync, this makes sure there will always be 1 replica with data up to date.

If the sync replica is slow or late to respond, we change it to be async and another one is picked to be sync instead.

### Asynchronous Replication 

Most of the time Asynchronous Replication is used, this gives a weaker durability.

If the leader fails before writing to a follower node, data is lost.

Nevertheless, this is a widely used approach.

## Setting Up New Followers

Setting new followers has to be done with no downtime as this would defeat the point of high availability.

Copying data from files doesn't work as database is always changing so copying would see different parts of database at different points in time.

Locking the db before copying 

The process goes as follows:
1. Take a consistent snapshot of the leader's database at some point in time preferably without having a lock (most databases have this feature as it is required for back up)
2. Copy the snapshot to the new database
3. Follower connects to the leader and requests all the data changes that happened since the snapshot 
   This requires that the snapshot is associated with an exact position in the leader's replication log
4. When the follower processes the backlog of data changes since the snapshot, we say it has **caught up** and we can continue to act normally
These steps could vary from one database to another.

## Handling Node Outages

### Follower Failure: Catch-up Recovery 

A follower keeps a log of data changes, if it crashes or it is restarted , the network can recover quire easily using the logs.

It can request changes from the leader and receive all the data changes since it disconnected.

### Leader failure: Failover

This is trickier as a new follower needs to be promoted to a leader, this process is called **failover**

Automatic failover consists of the following steps:
1. **Determing that the leader has failed**
   Most systems use a timeout, nodes bounce messages between each other, if a node doesn't respond for x seconds, it is assumed to be dead.
   
2. **Choosing a new leader**
   Could be done by an election process. The best candidate for leadership is usually the replica withthe most up-to-date data from the old leader.
   Getting all the nodes to agree on a new leader is **Consensus Problem** discussed in [[DDIA - Consistency and Consensus]]

3. **Reconfiguring the system to use the new leader**
   Clients needto send the writes to a new leader.
   If the old leader comes back, it might still thinm that it is the leader, so the system needs to ensure that the old leader knows that it is now a follower.
---
#### Failover Problems
- **Asynchronous Replication and Data loss**
  Most common solution is for data to be discarded, which may violate client's durability expectations 
- Two nodes can believe that they are the leader which is called **split brain**. If there are two nodes writing without a way to resolve conflicts, data is likely to be lost or corrupted
- **Choosing the correct timeout**
  Too long of a timeout means longer recovery.
  Too short of a timeout and it might result in unnecessary failover, maybe due to a temporary network or load increase
  If the system already has a high load, failover is likely to make this worse
Those problems make people resolve failover manually even if automatic failover is an option.

These issues are discussed more in [[DDIA - Consistency and Consensus]]

---

### Implementation of Replication Logs

#### Statement based replication 

The simplest way, the leader logs every INSERT, DELETE, UPDATE and sends them to the followers

This sounds reasonable, but can break down in many ways
- non deterministic results `RAND()`, `NOW()`
- statements with autoincrementing column or depend on already existing data, these need to be executed in the same order, which is hard to do if there is concurrency.
- Statements with side effects that may result in different behavior on each replica.

It's possible to work around these, but because there are so many edge cases, other replication methods are now preferred.

---
#### Write-ahead log (WAL) shipping

The log is an append only sequence of bytes that can be used to create a replica on another node.

Disadvantage of WAL is that it includes very low level details that it is often coupled to a storage engine.

If a db changes storage format, it might not even work with the same database

This might cause a problem.

If we want to update a single leader multiple follower system

We could update followers and use WAL to restore the data.

But if the follower changes db version or the db, WAL could be invalid.

So there would have to be some downtime during update.

