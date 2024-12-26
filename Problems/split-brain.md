Problem 1: How would you avoid the split-brain problem?
---
Imagine this situation 

A group of replica nodes are connected to a single primary instance

As any other conventional system,
Primary instance is accepting writes
Replica instances are accepting reads

Let's say there is a network failure because of which a small number of instances are not able to communicate to the primary instance, forming a second isolated group.

Now, the standard failover process would kick in for the isolated group which would promote one of the replica instances as the new primary.

Sometime later, let's say, the network failure is resolved now, which leads to having two primary instances in the ecosystem: one which was original primary instance and another because of failover.

This is a problem because two primary instances mean writes can be accepted at two entrypoints in the system. This can lead to conflicting writes and replica instances don't know which primary instance to follow.

This problem is called split-brain problem. In this problem, a group of nodes start taking actions as if it is an individual group in the ecosystem and eventually leads to conflicts or data inconsistencies.

This can happen anywhere and with any system in the world. 

Remember? You can't avoid partition as per CAP theorem. (though CAP is not the right tool, but just for argument sake here)

How would you avoid the split-brain problem?


### Solution: Mitigating the Split-Brain Problem

The split-brain problem can lead to data inconsistencies and conflicts. Below are strategies to mitigate or avoid this issue:

---

#### 1. **Quorum-Based Consensus**
- Implement a **quorum-based** approach for decision-making (e.g., using Raft or Paxos protocols).
- Writes are only allowed if a majority of nodes agree. If the network partitions, isolated groups without quorum cannot elect a new primary, preventing the creation of a second primary.
- **Trade-off**: This might reduce availability during partitions since operations require consensus from a majority.

---

#### 2. **Primary Lease with Expiry**
- The primary instance can hold a **lease** (a time-limited authority). 
- If the lease expires, the original primary stops accepting writes until it can renew the lease.
- Isolated nodes cannot elect a new primary until they verify that the original primary's lease has expired.
- **Caveat**: This requires accurate time synchronization across nodes (e.g., using NTP).

---

#### 3. **Fencing Tokens**
- Issue **monotonically increasing tokens** to the primary when it is elected.
- Any write requests to replica nodes must include the current token from the primary. Nodes accept writes only if the token matches the highest known token.
- If a split-brain occurs and an isolated group promotes a new primary, it will have a higher token. Once the network heals, conflicts can be detected and resolved based on token comparisons.

---

#### 4. **Witness or Tie-Breaker Node**
- Introduce an external **witness node** that acts as a tie-breaker during network partitions.
- The witness node has no actual data but tracks which group can retain the primary. Only the group with access to the witness can elect a new primary.
- **Limitation**: The witness itself can become a single point of failure unless designed with redundancy.

---

#### 5. **Causal Ordering and Conflict Resolution**
- Use **vector clocks**, **Lamport timestamps**, or other mechanisms to track causality between writes.
- In case of a split-brain, conflicts can be resolved during reconciliation based on these timestamps.
- **Example**: Systems like DynamoDB or Cassandra use these techniques to ensure eventual consistency.

---

#### 6. **Cluster Rejoin Policies**
- Define strict policies for how nodes rejoin the cluster after a network partition.
- When the network heals, the system can force one of the primaries to step down or reconcile the states before allowing the system to resume normal operation.

---

#### 7. **Avoiding Failover in Small Partitions**
- Introduce a threshold for failover: Failover processes should only trigger if a significant portion of the cluster is affected (e.g., more than 50% of nodes).
- Small isolated groups will remain read-only until they can reconnect to the primary.

---

#### 8. **Geo-Partitioning with Strong Leadership**
- Design the system to have a single source of truth for different geographical regions.
- Within a region, the leader manages writes, and cross-region replication is asynchronous or conflict-resolved through well-defined policies.

---

#### 9. **Use of External Coordination Services**
- Employ distributed coordination services like **Zookeeper**, **etcd**, or **Consul** to manage leader election and cluster state.
- These systems are designed to handle network partitions robustly and can enforce strict leadership invariants.

---

### Practical Example
In **Kafka**, split-brain prevention is handled by requiring a controller node to maintain a **Zookeeper session**. If the session is lost due to a partition, the controller steps down. Similarly, a broker only becomes a leader for a partition if it is part of the in-sync replicas (ISR) list, which is managed consistently by Zookeeper.

---

### Trade-Offs
While these strategies can help mitigate the split-brain problem, they often come with trade-offs in terms of availability, complexity, and performance. Designing a system to handle network partitions gracefully requires a clear understanding of the application's consistency and availability requirements.
