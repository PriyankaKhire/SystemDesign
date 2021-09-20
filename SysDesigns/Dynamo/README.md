<h1>Dynamo: Distributed Key - Value Store (AP - System)</h1>
  <p>It is a distributed key value store that is highly <b>available</b>, highly scalable and completely decentralised.</p>
  <p><b>Note</b>: Do not confuse Dynamo with DynamoDB</p>
  <h2>Motivation (For my understanding)</h2>
    <p>Amazon designed Dynamo, the motivation for designing a highly available system was customer satisfaction. <br/>
      Higher availability meant increased number of customers were served.
    </p>
    <p>So even if the system was imperfect it should always be available to the customers. <br/>
      Plus, inconsistencies can be resolved in the background, and most of the time they won't be noticed by the customers.
    </p>
  <h2>Non functional requirements</h2>
    <ul>
      <li><b>Scalability</b>: Throw more machines at the system to see improvements.</li>
      <li><b>Decentralized</b>: Cannot have a single point of failure.</li>
      <li><b>Eventual Consistency</b>: It will take time to make data consistent (replicate the data) across all nodes and that is ok.</li>
    </ul>
  <h2>APIs</h2>
    <h3>Put (Key, Value Object, Value Object Metadata)</h3>
      <p>Finds the node where the key-value pari should reside and then writes the data there. The metadata of the value object may contain versioning information along with other information.</p>
    <h3>Get (Key)</h3>
      <p>Finds the node where the value object is located, returns it along with the metadata associated with it. <br/>
        Depending on how you have configured Dynamo, the get operation can also return multiple versions of the value object.
      </p>
  <h2>Data Partitioning Criteria used</h2>
    <p>It applies <u>MD5 <a href="../../Basics/Data Partitioning Criteria/README.md#hash">hashing algorithm</a></u> on the Key to figure out which shard it should reside on. <br/>
      To tackle the problem of adding and removing new servers, it uses <b>Consistent Hashing.</b>
    </p>
  <h2>Node failures</h2>
    <p>If there are any node failures in the system, Dyanmo copies the data to all the healthy nodes in the cluster instead of following the majority quorum (N/2 + 1) rule. This is called <b><a href="../../Basics//Other Concepts/Quorum/README.md#sloppy-quorum">Sloppy Quorum</a></b>.</p>
  <h2>Inter-Node Communication</h2>
    <p>The nodes use <b><a href="../../Basics/Other Concepts/Gossip Protocol/README.md">Gossip Protocol</a></b> to keep track of each other. In this each node initiates a <code>Gossip Round</code> where it tells information about itself and other nodes it knows of to one other node. Eventually all the nodes learn about other nodes and if there is any node failure.</p>


