<h1>Lessons from Giant-Scale Services</h1>

[Paper Link](https://people.eecs.berkeley.edu/~brewer/papers/GiantScale-IEEE.pdf)

The paper discusses the fundamental model for services that need to be constantly available, focusing on the primary challenges they encounter in the real world: high availability, evolution, and growth.

<h2>Advantages of  giant-scale services</h2>

  - **Access Anywhere, Anytime:** enabling users to access services from various locations such as home, work, airports, etc.
  -  **Availability via Multiple Devices:** With most processing handled by the infrastructure, users can access services through devices like set-top boxes, network computers, and smartphones.
  - **Lower Overall Cost:** While difficult to quantify, infrastructure services offer a fundamental cost advantage over designs based on stand-alone devices. 
  - **Simplified Service Updates:** One of the most significant long-term advantages is the ability to upgrade existing services
    
  <h3>Components</h3>
  
  <img src="img/BasicModel.png"><br/>
  
  Assuming that the above system is read heavy. <br/>
  This basic model provides a framework for understanding the architecture of giant-scale sites, outlining the key components and their interactions to support the high demand and availability requirements of such services.<br/>
  Below are the components of the Basic Model:
  - **Clients:** Initiators of queries to the services, including web browsers, standalone email readers, or programs using XML and SOAP.
  - **IP Network:** Provides access to the service, whether it's the public Internet or a private network like an intranet.
  - **Load Manager:** balancing load among active servers.
  - **Servers:** Perform the actual work, combining CPU, memory, and disks into replicable units.
  - **Persistent Data Store:** Replicated or partitioned "database" spread across servers' disks, including network-attached storage like external DBMSs or RAID storage.
  - **Many services also use a backplane.** This optional system-area-network handles interserver traffic such as redirecting client queries to the correct server or coherence traffic for the persistent data store.
    
  <h3>Load Management</h3>
  While round-robin DNS balances load effectively, it doesn't hide inactive servers. Clients might continue to use down nodes until the DNS mapping expires, causing delays.
  <img src="img/Round-Robin.png"><br/>
  4-layer switches, these transport-layer switches understand TCP and port numbers, and can make decisions based on this information.
  Below image shows a 4-layer switch. The “clients” are actually other programs (typically Web servers) that use the smart-client approach to failover among different physical clusters, primarily based on load. 
  <img src="img/FourLayerSwitch.png">
  Because the persistent store is partitioned across servers, possibly without replication, node failures could reduce the store’s effective size and overall capacity. Furthermore, the nodes are no longer identical, and some queries might need to be directed to specific nodes. 
  This is typically accomplished using a layer-7 switch to parse URLs, but some systems, such as clustered Web caches, might also use the backplane to route requests to the correct node.4

<h2>Clusters in Giant-Scale Services</h2>
Clusters are collections of servers that work together on a single problem. Hardware failures and software faults are common occurrences due to the rapid evolution of systems. Clusters help simplify this issue by providing a solution. Basically they help with keeping the system available. Clusters should be able to grow services gradually to accommodate the uncertainty and cost associated with expanding a service.

We see server nodes as the fundamental components for giant-scale services. Our aim is to leverage their advantages, like independent faults and gradual scalability, to achieve our broader goals of ensuring availability, graceful degradation, and facilitating system evolution.

<h2>High Availability</h2>
  High availability is a major driving requirement behind giant-scale system design.
  <h3>Availability Metrics</h3>
  
  The traditional metric for availability is uptime of the web service.
  
  <code>uptime = (Mean time between failure - Mean time to reapir)/Mean time between failure</code>
  
  Following this equation, We can increase uptime by either reducing how often failures happen or by making fixes faster.

  <code>yield = queries completed/queries offered</code>

In practice, yield, is very similar to uptime numerically. However, it's more practical because it directly relates to user experience and accounts for the fact that not all seconds are equally valuable. For example, being offline for a second when there are no queries doesn't affect users or yield, but it lowers uptime. Also, being down for a second during peak and off-peak times might result in the same uptime, but significantly different yields due to varying levels of activity. Therefore, we prioritize measuring yield over uptime.

<code>harvest = data available/complete data</code>

<h3>DQ Principle</h3>

<code>Data per query × queries per second → constant</code>

<h3>Replication vs. Partitioning</h3>

Replication and partitioning are traditional techniques for increasing availability. 

When a fault occurs in a system using replication, the remaining replicas must handle the queries that were originally handled by the failed nodes. In a scenario where the system is already operating at high utilization (meaning it's handling a heavy load of queries), expecting the remaining replicas to take on the additional load becomes unrealistic.

While replicating data on disk itself might be relatively inexpensive, accessing and managing this replicated data require additional resources, often referred to as data quantity (DQ) points. On the other hand, partitioning involves dividing the data into smaller subsets and distributing them across different nodes. While partitioning may seem to save on storage space compared to replication, the real cost in terms of resource consumption, particularly DQ points, remains largely the same regardless of whether the database is replicated or partitioned.

**Choosing Between Replication and Partitioning:** While replication may offer advantages for managing heavy write traffic due to its benefits in data consistency and availability, the decision should be made based on a careful evaluation of the workload characteristics, system requirements, and associated costs.

**Replication stratergies:** Replication allows for varying levels of replication based on the importance of the data. Critical data that is essential for the system's operation may be replicated more extensively to ensure its availability and fault tolerance. In distributed systems, faults such as node failures are inevitable. Replication provides a mechanism to mitigate the impact of faults by ensuring that multiple copies of critical data are available across different nodes. Depending on the system's requirements, different replication strategies can be employed. For example, critical data may be replicated across multiple nodes, while less critical data may have fewer replicas or may not be replicated at all. 

<h3>Graceful Degradation</h3>

Graceful degradation refers to the ability of a system to maintain essential functionality and performance levels even when experiencing excessive load or unexpected faults. It involves managing system resources and priorities in such a way that critical services remain available and responsive, even under challenging conditions. 

<h4>Challenges with Capacity Management:</h4>

Managing system capacity in giant-scale systems is challenging due to the unpredictable nature of traffic fluctuations, the occurrence of single-event bursts, and the potential impact of faults on overall system capacity.

- **Peak-to-Average Ratio:** Giant-scale systems typically experience fluctuations in traffic, with peak usage significantly higher than average usage. This ratio, often ranging from 1.6:1 to 6:1, indicates the difference between the highest level of demand (peak) and the typical level of demand (average). Designing systems to handle these peaks can be costly, as it requires provisioning capacity well above the average load to accommodate sudden spikes in traffic.
- **Single-Event Bursts:** Certain events, such as online ticket sales for popular movies or events, can cause an extraordinary surge in traffic that far exceeds the average load. Even with substantial increases in capacity, these sudden bursts can overload the system, leading to service disruptions or slowdowns. For instance, despite adding ten times the normal capacity, moviephone.com still struggled to handle the surge in ticket sales for "Star Wars: The Phantom Menace."
- **Impact of Faults:** Some types of faults, such as power outages or natural disasters, can have a significant impact on system capacity. In such situations, the remaining operational nodes may become saturated as they try to compensate for the lost capacity, leading to degraded performance or service outages.

These factors necessitate the implementation of strategies for graceful degradation and effective resource allocation to maintain high availability despite fluctuations in demand and unforeseen disruptions.

<h4>Strategies for Graceful Degradation:</h4>

- **Admission Control (AC):** By limiting the number of incoming queries based on estimated query cost (measured in DQ), systems can reduce the average data required per query, thereby increasing capacity.
- **Priority- or Value-Based AC:** Systems can prioritize certain types of queries over others based on their importance or value, ensuring that critical services are given precedence during periods of high demand.
- **Reduced Data Freshness:** Under saturation, systems can reduce the frequency of data updates to decrease the workload per query. While this sacrifices data freshness, it increases system capacity and responsiveness, ensuring that critical services remain available.
