<h1>Relational Database Management System</h1>
  <p>The data is presented to the user in the form of tables known as relations. 
  The data is arranged as the collection of rows and columns. <br/>
  Structured query language (SQL) is implemented by most commercial RDBMS systems for accessing the database. <br/>
  Indices can be created on the tables, that helps in faster data retrieval. However, creating indices can be expensive. 
  </p>
  <p>Relational database design satisfies the ACID (atomicity, consistency, integrity and durability) properties required from a database design. <br/></p>
  <p>It has the following properties:</p>
  <ul>
    <li><b>Relations and attributes:</b> The tables represent entities, and the attributes represent the properties of the respective entities.</li>
    <li><b>Primary keys:</b> The attribute or set of attributes that help in uniquely identifying a record.</li>
    <li><b>Relationships:</b> The relationships between the various tables are established with the help of foreign keys. Foreign keys are attributes occurring in a table that are primary keys of another table.
        The types of relationships between the tables are following:
        <ul>
          <li>one to one</li>
          <li>one to many</li>
          <li>many to many</li>
        </ul>
    </li>
  </ul>
  <h2>In system design</h2>
    <p>They are very well suited for storing and querying structured relational data. <br/>
    They are typically characterized as CA systems that therefore sacrifice partition tolerance, and are often implemented as a single server.
    </p>
  <h3>MySQL</h3>
    <p>By default, MySQL is CA system, because it follows master, slave approach. In this the data is replicated to the slaves. In case if some of the slaves lose connection to their master (or if the master goes down), the slaves elect a new master. Thus we now have 2 masters, each with their own set of slaves. </p>
    <p>However, MySQL also has another configuration which is a clustered configuration. It prioritizes CP over availability eg. the cluster will shutdown if there are not enough live nodes to serve all the data.</p>
  <h2>Definitions</h2>
    <h3>Primary Key</h3>
      <p>A primary key is a column (or set of columns) in a table whose values uniquely identify a row in the table. </p>
      <p>Primary key cannot be NULL.</p>
    <h3>Foreign key</h3>
      <p>Foreign keys are primary keys of another table.</p>
      <p>In order to add a row with a given foreign key value, there must exist a row in the related table with the same primary key value.</p>
  