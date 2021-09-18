<h1>Data Partitioning Criteria</h1>
  It is based on how the shard key is assigned to each shard.
  <h2>Range</h2>
    The shard is selected based on what range the key falls into.
    <h3>Example</h3>
      If your shard key is user name then you can put all users whose user name starts with A on one shard and so on.<br/>
      Another example could be, if your shard key is store id, then you can decide to put all ids less than 5 on one shard, ids between 5-10 on another shard and so on.
    <h3>Pros</h3>
      Makes sequential access very fast, because data that is "close" in the given range will be on the same shard.
    <h3>Cons</h3>
      Due to the unpredictable nature of the data, one shard might hold more data than other.<br/>
      For example we might have many users with username that starts with letter A than letter Z.
  <h2>List</h2>
    The shard key is made part of a list. So all the shard keys that fall in that list go reside on one shard.<br/>
    List partitioning is similar to range partitioning in many ways, however in list based the items selected in the list may or may not be contiguous. 
    <h3>Example</h3>
      If the shard key is Country, then we can add Iceland, Norway, Sweden, Finland, Denmark to Nordic Country list,
      and put their data on one shard.
  <h2>Hash</h2>
    <p>In this scheme we have a hash function, that maps the shard keys to shards.</p>
    <h3>Example</h3>
      <p>The profile pages of celebrities get substantially more traffic than the average user, so a hash function can be used to randomly distribute these celebrity users and prevent hotspots.</p>
    <h3>Pros</h3>
      <ul>
        <li>The data gets evenly distributed.</li>
        <li>No single point of failure, any server who knows the hash function can run it and start assigning shards.</li>
      </ul>
    <h3>Cons</h3>
      <ul>
        <li>Adding new servers can get tough, to address this issue we can use consistent hashing.</li>
      </ul>
  <h2>Round Robin</h2>
    <p>The simplest of them all, if there are <code>n</code> shards then the <code>i th</code> row of data is assigned the shard <code>i % n</code></p>
    <h3>Pros</h3>
      <ul>
        <li>Sequential access of data is easy.</li>
      </ul>
    <h3>Cons</h3>
      <ul>
        <li>Adding new servers redistributes the data.</li>
      </ul>
  <h2>Name Node</h2>
    <p>It keeps the directory tree of all files in the file system, and tracks where across the cluster the file data is kept. Sort of like indexing. <br/>
      Client applications talk to the NameNode whenever they wish to locate a file, or when they want to add/copy/move/delete a file. The NameNode responds the successful requests by returning a list of relevant DataNode servers where the data lives.
    </p>
    <h3>Pros</h3>
      <ul>
        <li>You can control where the data is being placed</li>
        <li>Avoids hot spots.</li>
        <li>Even data distribution, may require extra logic for this.</li>
      </ul>
    <h3>Cons</h3>
      <ul>
        <li>Single point of failure, you can address this by adding a second server.</li>
        <li>Additional latency required to look up the shard.</li>
      </ul>
<h1>Good Reads</h1>
  <a href="https://medium.com/must-know-computer-science/system-design-sharding-data-partitioning-b7201596aafa">System Design — Sharding / Data Partitioning</a><br/>
  <a href="https://en.wikipedia.org/wiki/Partition_(database)">Wikipedia - Partition (database)</a><br/>
  <a href="https://igotanoffer.com/blogs/tech/sharding-system-design-interview">Sharding: system design interview concepts (7 of 9) from igotanoffer</a><br/>
  <a href="https://cwiki.apache.org/confluence/display/HADOOP2/NameNode">NameNode</a>