<h1>Memcached</h1>
  <p>It is distributed memory object caching system. 
    It is an <b>in-memory key-value store</b> for small chunks of arbitrary data (strings, objects) from results of database calls, API calls, or page rendering.<br/>
    When the memory gets full, it uses LRU based eviction policy. <br/>
    It does not support replication. <br/>
    Memcached server is a big hash table. The max length of the key is 250bytes.
  </p>
  <h2>Architecture</h2>
    <p> All the clients know about all the memcached servers. The clients access the servers over TCP or UDP connections.
      The servers are independent of one another. 
    </p>
  <h2>Advantages</h2>
    <ol>
      <li>Memcached’s uses include caching and storing session data.</li>
      <li>The system where memcached is deployed can achieve high performance and scalability.</li>
      <li>It is very good at handling high traffic websites. It can read lot of information at a time and have a great response time.</li>
      <li>Excellent for high latency queries, since caching can prevent the need to repeat the big units of work.</li>
    </ol>
  <h2>Good Reads</h2>
  https://gigadom.in/tag/memcached/  <br/>
  https://dzone.com/articles/memcached-a-distributed-memory-object-caching-syst
  