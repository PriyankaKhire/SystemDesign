<h1>MongoDB</h1>
  <p>It is a NoSQL database that stores data as BSON (binary JSON) documents. <br/>
    Relative to the CAP theorem, MongoDB is a CP data store—it resolves network partitions by maintaining consistency, while compromising on availability. <br/>
  </p>

  <h2>Architecture</h2>
    <p>MongoDB is a single-master system. Each replica set (or master slave cluster) can only have one master. The master takes in all the write operations, we can even read from the master. However, we can configure the DB to read from slaves as well.<br/>
      When the master dies, the slave with the most recent operation log will be elected as the new master. Once all the other slaves catch up with the new master, the cluster becomes available again.<br/>
      As clients can't make any write requests during this interval, the data remains consistent across the entire network.
    </p>

  <h2>Advantages</h2>
    <ul>
      <li>Flexible Document Schemas: The fields of one document can differ from another.<br/> <b>Pretty great for horizontal scaling.</b></li>
      <li>Easily accessible data: since the data is stored in documents, you can access the data from any language, in data structures that are native to that language.<br/> Example: dictionaries in Python, associative arrays in JavaScript, Maps in Java, etc</li>
      <li><b>There’s no downtime required to change schemas</b>, and you can start writing new data to MongoDB at any time, without disrupting its operations.</li>
      <li>Ease of scale-out − MongoDB is easy to scale.</li>
    </ul>

  <h2>Good Reads</h2>
    https://www.ibm.com/cloud/learn/cap-theorem
    https://www.mongodb.com/advantages-of-mongodb