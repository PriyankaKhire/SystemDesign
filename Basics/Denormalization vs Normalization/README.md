<h1>Data Denormalization</h1>
  <p>In this technique we <b>add redundant data to one or more tables.</b>
    This can help us avoid costly joins in a relational database.
  </p>
  <h2>Example 1</h2>
    <p>For example, in a normalized database, we might have a Courses table and a Teachers table. Each entry in Courses would store the teacherID for a Course but not the teacherName. When we need to retrieve a list of all Courses with the Teacher’s name, we would do a join between these two tables. <br/><br/>
          In some ways, this is great; if a teacher changes his or her name, we only have to update the name in one place. 
          The drawback is that if tables are large, we may spend an unnecessarily long time doing joins on tables. 
          Denormalization, then, strikes a different compromise. Under denormalization, we decide that we’re okay with some redundancy and some extra effort to update the database in order to get the efficiency advantages of fewer joins. 
    </p>
  <h2>Example 2</h2>
    <p>Let’s think about where to store data for messages between two friends. Both users want fast responses, but what if their user records are split up onto different shards? We can store the messages in both places. This increases write time slightly, because it means the data is written twice. But it also ensures the message data is fast to look up regardless which  friend is checking their messages.</p>
  <h2>Pros</h2>
    <ul><li>Retrieving data is faster since we do fewer joins</li></ul>
  <h2>Cons</h2>
    <ul>
      <li>Updates and inserts are more expensive.</li>
      <li>The duplicated data may be inconsistent </li>
    </ul>
  <h2>Uses</h2>
    <ul>
      <li>We mainly use data denormalization during sharding.</li>
      <li><b>De-normalization is needed if you are avoiding joins.</b></li>
      <li>For web scale services that have to process loads higher than any database can scale to, denormalization of data helps to retrieve data faster.</li>
      <li>Many mega-scale websites with billions of records, petabytes of data, many thousands of simultaneous users, and millions of queries a day are doing is using a sharding scheme and some are even advocating denormalization as the best strategy for architecting the data tier.</li>
    </ul>
  <h2>Companies using Data Denormalization</h2>
  <ul>
    <li>Flickr decided to denormalize because it took 13 Selects to each Insert, Delete or Update.
      <p>Flickr shards and replicates their data to reach high performance levels. For Flickr this moves transaction logic back into their application layer, but the win is higher scalability.</p>
    </li>
    <li>Ebay who moved all significant functionality out of the database and into applications.
      <p>Ebay maintains data integrity through a service layer that encapsulates all data access. The service layer handles referential integrity, managing replicated copies, doing joins, and so on. It's more error prone than having the database do all this work, but you are able to do scale past what even the highest end databases can handle.</p>
    </li>
  </ul>
<h1>Data Normalization</h1>
  <p>Structuring the data (in a relational DB) so that we try to reduce the data redundancy.</p>
  <p>We normalize data to prevent anomalies. 
    Anomalies are bad things, like forgetting to update someone's address in all the places it's been stored when they move.
    This anomaly happened because the address had been duplicated. So to prevent the anomaly we don't duplicate data. 
    We split everything up, so it's stored once and exactly once. 
  </p>
  <p>We have a bunch of rules to follow to normalize our data. 
    The price of normalization is that when we want a person's address we have to go find the person, and their address in separate operations and bring the data together again. 
    This is called a join.
  </p>
  <h2>Pros</h2>
    <ul>
      <li>The data is consistent since we only have to update it once.</li>
    </ul>
  <h2>Cons</h2>
    <ul>
      <li>To get all the data we have to perform joins and they are relatively slow, especially over very large data set.</li>
    </ul>
  <h2>Anomalies</h2>
    <p>If the data has not been normalized it can lead to following 3 anomalies</p>
    <ol>
      <li><b>Update anomaly</b>:  The same information can be expressed on multiple rows; therefore updates to the relation may result in logical inconsistencies. </li>
      <li><b>Insertion anomaly</b>: There are circumstances in which certain facts cannot be recorded at all. <i>For example</i>, each record in a "Faculty and Their Courses" relation might contain a Faculty ID, Faculty Name, Faculty Hire Date, and Course Code. Therefore, the details of any faculty member who teaches at least one course can be recorded, but a newly hired faculty member who has not yet been assigned to teach any courses cannot be recorded, except by setting the Course Code to null. This phenomenon is known as an insertion anomaly.</li>
      <li><b>Deletion anomaly</b>: Sometimes deleting one set of data may lead us to delete some other data that represents something else due to the cascading effect. <i>For example</i> The "Faculty and Their Courses" relation described in the previous example suffers from this type of anomaly, for if a faculty member temporarily ceases to be assigned to any courses, the last of the records on which that faculty member appears must be deleted, effectively also deleting the faculty member, unless the Course Code field is set to null. This phenomenon is known as a deletion anomaly.</li>
    </ol>
<h1>Good Reads</h1>
  <ul>
    <li><a href="http://highscalability.com/blog/2007/8/16/scaling-secret-2-denormalizing-your-way-to-speed-and-profit.html">Denormalizing Your Way To Speed And Profit - Article by high scalability</a> </li>
    <li><a href="https://en.wikipedia.org/wiki/Database_normalization">Database normalization - Wikipedia</a></li>
  </ul>
