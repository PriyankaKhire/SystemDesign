<h1>ACID Theorem</h1>
  <p>Transactions are series of Database operations.</p>
  <p>A database is said to be ACID compliant when it follows the following properties:</p>
  <ul>
    <li><b>Atomicity</b> :- All operations succeed or fail together.</li>
    <li><b>Consistency</b> :- A successful transaction places the database in a valid state, that is, no schema violations</li>
    <li><b>Isolation</b> :- Transactions can be executed concurrently </li>
    <li><b>Durability</b> :- Completed transactions persist, even when servers restart etc.</li>
  </ul>
  <p>Usually relational databases are ACID compliant. <br/>
    So the database is consistent after every transaction.
  </p>

<h1>BASE Theorem</h1>
  <p>A database is said to be BASE compliant when it follows the following properties:</p>
  <ul>
    <li><b>B</b>asic <b>A</b>vailability :- The system is guaranteed to be available in event of failure.</li>
    <li><b>S</b>oft-state :- The state of the data could change without application interactions due to eventual consistency.</li>
    <li><b>E</b>ventual consistency :- The system will be eventually consistent after the application input. The data will be replicated to different nodes and will eventually reach a consistent state. But the consistency is not guaranteed at a transaction level.</li>
  </ul>
  <p>This essentially means that: It’s OK to use stale data, and it’s OK to give approximate answers. <br/>
     BASE providesless assurance than ACID, but it scales very well and reacts well to rapid data changes.
  </p>

<h1>Good Reads</h1>
  https://dba.stackexchange.com/questions/31260/consistency-in-acid-and-cap-theorem-are-they-the-same <br/>
  https://www.johndcook.com/blog/2009/07/06/brewer-cap-theorem-base/ <br/>
  https://www.dataversity.net/what-is-base/
  