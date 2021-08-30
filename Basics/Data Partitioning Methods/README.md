<h1>Data Partitioning Methods</h1>
  <p>Here we break data into smaller parts, so it's easy to access it. Thus, we are trying to distribute the traffic as evenly as possible.<br/>
    There are 2 types of data partitioning methods.
    <ul>
      <li>Horizontal Partitioning (Sharding)</li>
      <li>Vertical Partitioning</li>
    </ul>
  </p>
  <h3>Example Table</h3>
    <p>
    Let us imagine there is the following table: <br/>
      <table>
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Email</th>
          <th>isActive?</th>
      </tr>
        <tr>
          <td>1</td>
          <td>a</td>
          <td>a@domain.com</td>
          <td>Y</td>
        </tr>
        <tr>
          <td>2</td>
          <td>b</td>
          <td>b@domain.com</td>
          <td>N</td>
        </tr>
        <tr>
          <td>3</td>
          <td>c</td>
          <td>c@domain.com</td>
          <td>Y</td>
        </tr>
        <tr>
          <td>4</td>
          <td>d</td>
          <td>d@domain.com</td>
          <td>N</td>
        </tr>
        <tr>
          <td>5</td>
          <td>e</td>
          <td>e@domain.com</td>
          <td>Y</td>
        </tr>
      </table>
    </p>
  <h2>Horizontal Partitioning (Sharding)</h2>
    <p>In this strategy, each partition is a separate data store, but all partitions have the same schema.</p>
    <h3>Example Table</h3>
    <p> In horizontal partitioning the table would look like following: <br/>
      Partitioned based on isActive
      <table>
        <tr>
          <td>
            <table>
              <tr>
                <th>ID</th>
                <th>Name</th>
                <th>Email</th>
                <th>isActive?</th>
              </tr>
              <tr>
                <td>1</td>
                <td>a</td>
                <td>a@domain.com</td>
                <td>Y</td>
              </tr>
              <tr>
                <td>3</td>
                <td>c</td>
                <td>c@domain.com</td>
                <td>Y</td>
              </tr>
              <tr>
                <td>5</td>
                <td>e</td>
                <td>e@domain.com</td>
                <td>Y</td>
              </tr>
            </table>
          </td>
          <td>
            <table>
              <tr>
                <th>ID</th>
                <th>Name</th>
                <th>Email</th>
                <th>isActive?</th>
              </tr>
              <tr>
                <td>2</td>
                <td>b</td>
                <td>b@domain.com</td>
                <td>N</td>
              </tr>
              <tr>
                <td>4</td>
                <td>d</td>
                <td>d@domain.com</td>
                <td>N</td>
              </tr>
            </table>
          </td>
        </tr>
      </table>
    </p>
    <h3>Pros</h3>
      <ul>
        <li>The DB can handle more traffic because any one server in the cluster only has to respond to a fraction of the total requests.</li>
        <li>Quickly scaling by adding more shards</li>
        <li>Better performance because each machine is under less load</li>
      </ul>
    <h3>Cons</h3>
      <ul>
        <li>The queries become more complex because they have to somehow get the correct shard key</li>
        <li>Implementation can get more complex</li>
        <li>Redundant data due to denormalization.</li>
      </ul>
  <h2>Vertical Partitioning</h2>
    <p> In this strategy, each partition holds a subset of the fields for items in the data store. 
      The fields are divided according to their pattern of use.<br/>
      Meaning, different properties of an item are stored in different partitions. <br/>
      For example, frequently accessed fields might be placed in one vertical partition and less frequently accessed fields in another.<br/><br/>
      A common form of vertical partitioning is to split static data from dynamic data, since the former is faster to access than the latter, particularly for a table where the dynamic data is not used as often as the static.
    </p>
    <h3>Example Table</h3>
      <p> In vertical partitioning the table would look like following:
        Partitioning based on ID, Name and email, isActive.
        <table>
          <tr>
            <td>
              <table>
                <tr>
                  <th>ID</th>
                  <th>Name</th>
                </tr>
                <tr>
                  <td>1</td>
                  <td>a</td>
                </tr>
                <tr>
                  <td>2</td>
                  <td>b</td>
                </tr>
                <tr>
                  <td>3</td>
                  <td>c</td>
                </tr>
                <tr>
                  <td>4</td>
                  <td>d</td>
                </tr>
                <tr>
                  <td>5</td>
                  <td>e</td>
                </tr>
              </table>
            </td>
            <td>
              <table>
                <tr> 
                  <th>Email</th>
                  <th>isActive?</th>
                </tr>
                <tr>
                  <td>a@domain.com</td>
                  <td>Y</td>
                </tr>
                <tr>
                  <td>b@domain.com</td>
                  <td>N</td>
                </tr>
                <tr>
                  <td>c@domain.com</td>
                  <td>Y</td>
                </tr>
                <tr>
                  <td>d@domain.com</td>
                  <td>N</td>
                </tr>
                <tr>
                  <td>e@domain.com</td>
                  <td>Y</td>
                </tr>
              </table>
            </td>
          </tr>
        </table>
      </p>
    <h3>Pros</h3>
      <ul>
        <li>Vertical partitioning is straightforward to implement and has a low impact on the application.</li>
        <li>Relatively slow-moving data (product name, description, and price) can be separated from the more dynamic data (stock level and last ordered date)</li>
        <li>Sensitive data can be stored in a separate partition with additional security controls.</li>
      </ul>
    <h3>Cons</h3>
      <ul>
        <li>If our application experiences additional growth, then it may be necessary to further partition a feature specific DB across various servers (e.g. it would not be possible for a single server to handle all the metadata queries for 10 billion photos by 140 million users).</li>
      </ul>
    <h3>Use Cases</h3>
      <p> It is ideally suited for column-oriented data stores such as HBase and Cassandra. <br/>
        If the data in a collection of columns is unlikely to change, you can also consider using column stores in SQL Server.
      </p>
  <h2>Good Reads</h2>
    <ul>
      <li><a href="https://nerohoop.gitbooks.io/system-design/content/system-design-and-scalability/database-denormalization-and-nosql.html">Gitbooks system design.</a> ⭐ </li>
      <li><a href="https://docs.microsoft.com/en-us/azure/architecture/best-practices/data-partitioning">Horizontal, vertical, and functional data partitioning - Microsoft</a> </li>
      <li><a href="https://igotanoffer.com/blogs/tech/sharding-system-design-interview">Sharding: system design interview concepts - igotanoffer</a> </li>
    </ul>
  
  
  
  