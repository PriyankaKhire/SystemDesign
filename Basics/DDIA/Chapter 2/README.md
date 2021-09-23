<h1>Chapter 2: Data Models and Query Languages</h1>
  <h2>Introduction</h2>
    <p>
      Most applications are built by layering one data model on top of another. <br/>
      The developers look at the real world and chose to represent the data in OOP manner, in terms of objects and classes. <br/>
      The hardware developers found a way to represent bytes in terms of electric current, pulses of lights, magnetic fields etc.
    </p>
    <p>The core ideology is the same: each layer hides the complexity of the layer below it by providing clean data model.</p>
  <h2>Relational Model Vs Document Model</h2>
    <p>The best known relational model is SQL: data is organized into relations (called tables), and where each relation is unordered collection of tuples (rows).</p>
    <h3>The Birth of NoSQL</h3>
      <p>Reasons for adoption of NoSQL:</p>
      <ul>
        <li>Greater scalability than relational DB.</li>
        <li>Higher write throughput.</li>
        <li>It's open source</li>
        <li>Specialized queries that are not supported by relational DB.</li>
        <li>More dynamic and expressive data model. It's more versatile than relational DB.</li>
      </ul>
    <h3>The Object-Relational Mismatch</h3>
      <p><b>Impedance Mismatch</b>: Most applications are developed in OOP languages. The programs store the data in form of objects. To store this data in a relational DB, you require a wrapper of some sort that translates application code objects into relational tables, rows and columns. This disconnect is called impedance mismatch.</p>
      <p>So when you do look at data it is very important to chose how you will represent it in DB, if you will use JSON or XML etc.</p>
      <p>Generally JSON is simpler than XML. Many document oriented DBs like MongoDB, RethinkDB, CouchDB and Espresso use JSON.</p>
      <p>JSON also queries better.</p>
    <h3>Many-to-One and Many-to-Many Relationships</h3>
      <p>Why are ENUMS important ?</p>
      <p>Because, if the data associated with it changes, we just have to make this change in one place rather than multiple places. In all these multiple places where this data is going to be used, we just fill out the ENUM ID.</p>
      <p>This is also one of the biggest reasons why we <I>Normalize</I> data in DB so, we don't have to change it in multiple places in case it changes.</p>
    <h3>Are Document Databases Repeating History?</h3>
      <p><b>Hierarchical Model</b>: Represents data as tree of records, and records nested within records.<br/>
        This model worked well with one-to-many relationship but made many-to-many relationship difficult.
      </p>
      <p>Various solutions were proposed to solve limitations of hierarchical model: Relational Model and Network Model.</p>
      <h4>The network model</h4>
        <code>Not important, not going to make notes for this one.</code>
      <h4>The relational model</h4>
        <code>Not important, not going to make notes for this one.</code>
      <h4>Comparison to document databases</h4>
        <code>Not important, not going to make notes for this one.</code>
    <h3>Relational Vs Document Databases Today</h3>
      <p><b>Document data model pros</b>: schema flexibility, better performance, for some applications it is closer to the data structures used by the applications.</p>
      <p><b>Relational data model pros</b>: better support for joins, many-to-one and many-to-many relationships.</p>
      <h4>Which data model leads to simpler application code?</h4>
        <p>If your data has document like structure like tree of one-to-many relationships, then use document model.</p>
        <p><b>Limitations of document model</b>: cannot directly access nested file item. This model is not suitable for applications that use many-to-many relationships.</p>
      <h4>Schema flexibility in the document model</h4>
        <code>Not important, not going to make notes for this one.</code>
      <h4>Data locality for queries</h4>
        <code>Not important, not going to make notes for this one.</code>
      <h4>Convergence of document and relational databases</h4>
        <code>Not important, not going to make notes for this one.</code>
  <h2>Query Languages for Data</h2>
    <code>At this point I am thinking, is this all even really important ? I have done too many system design interviews to know that they won't ask me this much details at my level.</code>