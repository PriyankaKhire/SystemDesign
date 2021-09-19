<h1>Quorum</h1>
  <p>In Distributed Systems, data is replicated across multiple servers for fault
    tolerance and high availability. Once a system decides to maintain multiple
    copies of data, another problem arises: how to make sure that all replicas are
    consistent, i.e., if they all have the latest copy of the data and that all clients
    see the same view of the data?</p>
  <p>Quorum is the minimum number of nodes where the distributed operation (such as replicating data) needs to be done successfully.</p>
  <h2>What value should we choose for a quorum?</h2>
    <p>If the cluster has <code>N</code> nodes then <code>(N/2)+1</code> i.e. more than half the nodes in the cluster.</p>
  <h2>Sloppy Quorum</h2>
    <p>All reads and writes are done on first <code>N</code> healthy nodes</p>
<h1>Good Reads</h1>
  <a href="https://en.wikipedia.org/wiki/Quorum_(distributed_computing)#:~:text=A%20quorum%20is%20the%20minimum,operation%20in%20a%20distributed%20system.">Quorum (distributed computing)</a><br/>
  <a href="https://medium.com/@sunny_81705/quorum-in-distributed-systems-37cbe17aae88">Quorum In Distributed Systems</a>