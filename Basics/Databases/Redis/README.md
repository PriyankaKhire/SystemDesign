<h1>Redis</h1>
  <p> Redis stands for <b>RE</b>mote <b>DI</b>ctionary <b>S</b>erver. <br/>
    It is not just a plain key-value store, it is actually <i>a data structures server</i>, supporting different kinds of values.
    In Redis, the value (of the key-value pari) is not limited to a simple string, but can also hold more complex data structures. <br/><br/>
    It is used primarily as an application cache or quick-response database. Because it stores data in memory, rather than on a disk or solid-state drive (SSD), Redis delivers unparalleled speed, reliability, and performance.
  </p>

  <h2>Architecture</h2>
    <p>Redis is AP system. <br/>
      Redis uses Master-Salve approach. The slaves are the exact copies of the master.
      The slave will always try to reconnect to the master every time the link breaks, and try to be an exact replica of the master <i>regardless</i> of what ever happens to the master.<br/>
      The best configuration for Redis Master-Slave is to perform Writes to the master and reads from the salve.<br/>
      If the master goes down, we can add a new machine as the master or pick one of the slaves as the new master.<br/>
      The data replication is down asynchronously. 
    </p>

  <h2>Advantages</h2>
    <ol>
      <li>Faster response times when performing read and write operations.</li>
      <li>Redis can queue tasks that may take web clients longer to process than usual.</li>
      <li>Because Redis supports the use of publish and subscribe (Pub/Sub) commands, users can design a high-performance chat and messaging services across all their applications and services.</li>
      <li>High Availability</li>
  </ol>

  <h2>Good Reads</h2>
  https://medium.com/@sunny_81705/redis-master-slave-architecture-e730403cb495
  https://www.ibm.com/cloud/learn/redis