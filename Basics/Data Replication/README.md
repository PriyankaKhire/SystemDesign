# Data Replication
Replication is copying data from a host machine to multiple destination machines connected via a network. Basically, replication is the process of storing data in more than one place. Every node that stores a copy of the data is called a **replica**.

## Advantages
- **High availability:** By storing the copies of data in multiple host machines, the database is resilient to individual host machines' failures and can safely redirect the reads and writes to live host machines.
- **Load distribution:** The database can redistribute the read and write queries to multiple host machines, thus preventing the load on single host machines.
- **Reduced geographic latency:** The replicated host machines can be placed geographically closer to the user, thus reducing the latency on the end user.

## Types of Replication
- **Full replication:**  We replicate the complete data from one host machine to another. 
- **Transactional replication:** 
  1. A full copy of the data is replicated from the initial source to the destination.
  2.  Once the destination replica is up to date, it only receives updates from the source when the data is modified.
- **Snapshot replication:** Snapshot replication is a replication where the replication software takes a point-in-time image of the data and replicates it from source to destination.
- **Merge replication:** Merge replication is a replication where individual replicas of the databases work autonomously to receive reads and writes. This data is later converged and merged into a single, uniform result. 
- **Key-based incremental replication:** Key-based incremental replication scans the database for modified keys and only replicates data for those keys.

## Challenges of replication
- Replicating data across multiple host machines means that maintaining data consistency across multiple hosts becomes a prime requirement. 
- When there's a network problem between copies of the same data, it can cause delays in updating the copies and, in the worst case, make the data unavailable. This is because the copies can't communicate with each other effectively.

## Replication Strategies 
### Single leader replication (Master slave)
- The database selects one of the replicas as the leader. The client should send every modification request to the leader replica that writes it to its local storage engine.
- Other replicas are called followers. Once the leader acknowledges the modification request, follower replicas synchronize the data from the leader through the replication log. The follower then persists the data changes to its local storage engine.

### Multi-leader Replication
- There are multiple leaders receiving writes.
- Each leader simultaneously acts as a follower to other leaders.

Multileader replication finds its application in multi-data center setups that include a leader and followers in each data center. The client connects to the leader of a particular data center for write requests. Similarly, the client connects to the leader or followers of the data center to read data. 


