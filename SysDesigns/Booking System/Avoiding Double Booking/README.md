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
  <h2>Good Reads</h2>
    <ul>
      <li><a href="https://medium.com/airbnb-engineering/avoiding-double-payments-in-a-distributed-payments-system-2981f6b070bb">Avoiding Double Payments in a Distributed Payments System</a></li>
    </ul>