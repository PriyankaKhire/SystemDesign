<h1>Avoiding Double Booking</h1>
  <p>There are 2 scenarios that can cause double booking.</p>
  <ol>
    <li>User clicks on the book button multiple times</li>
    <li>Multiple users try to book same resource (room in hotel or same movie seat) at the same time.</li>
  </ol>
  <p>Let us take a closer look at both of these scenarios.</p>
  <h2>User clicks on Book Button multiple times</h2>
    <img src="img/User Clicks Twice.jpeg" height="500px">
    <h3>Solution</h3>
      <ul>
        <li>You can grey out the <i>Book Button</i> after the first time the client clicks on it.</li>
        <li>Use Idempotent APIs</li>
      </ul>
      <h4>Idempotent API</h4>
        <p>For an API request to be idempotent, clients can make the same call repeatedly and the result will be the same. In other words, making multiple identical requests should have the same effect as making a single request.</p>
        <p><b>HTTP PUT</b> request is idempotent.</p>
        <p>We can introduce an <i>idempotent key</i> in the API request call.</p>
        <img src="img/Idempotent transaction.jpeg" height="500px">
        <h5>Which Idempotent key to choose ?</h5>
          <p>We can choose a primary key of the table where we are going to store the transactions, that way we move the computation of generating a unique key to the database server.</p>
  <h2>Multiple users try to book the same resource.</h2>
    <img src="img/Multiple Users Book Same Room.png">
    <p>We can avoid this issue by database locking mechanism</p>
    <h3>Pessimistic locking</h3>
      <p>Prevents simultaneous updates by placing a <b>lock on a record</b> that is being updated.</p>
      <ul>
        <li>Pros</li>
        <ul>
          <li>Easy to implement because it serializes the updates</li>
        </ul>
        <li>Cons</li>
        <ul>
          <li>Deadlocks may occur if multiple records are locked.</li>
          <li>Transactions could be locked for a longer time, thus this approach is not that scalable.</li>
        </ul>
      </ul>
    <h3>Optimistic locking</h3>
      <p>This approach allows multiple users to update the same record by maintaining either a version number or time-stamp. </br> Generally version number approach is preferred because system clock can be inaccurate overtime.</p>
      <p>A new column called version is added to the DB table.</p>
      <img src="img/Optimistic Locking.png">
      <ul>
        <li>Pros</li>
        <ul>
          <li>We don't need to lock DB resources.</li>
        </ul>
        <li>Cons</li>
        <ul>
          <li>When there are multiple concurrent requests the performance can go down.</li>
        </ul>
      </ul>
    <h3>Database constraints</h3>
      <p>The simplest of them all, we can just add constraints to the DB table. So every time a user is trying to update a record, even if there is another transaction, that is taking place with that record, just because of adding the constraint, the new transaction will check for the constraint on that record again before updating.</p>
      <p>Check out <a href="https://www.w3schools.com/sql/sql_constraints.asp">Constraints tutorial</a> for more information.</p>
      <ul>
        <li>Pros</li>
          <ul>
            <li>Easy to implement</li>
          </ul>
        <li>Cons</li>
         <ul>
           <li>Users could see "rooms available" but when they try to book it, they could get response "no rooms available"</li>
           <li>Not all databases support constraints.</li>
         </ul>
      </ul>
  <h2>Good Reads</h2>
    <ul>
      <li><a href="https://medium.com/airbnb-engineering/avoiding-double-payments-in-a-distributed-payments-system-2981f6b070bb">Avoiding Double Payments in a Distributed Payments System</a></li>
    </ul>