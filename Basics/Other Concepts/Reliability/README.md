<h1>What is Reliability ?</h1>
  <ol>
    <li>Durability: Data must not be lost</li>
	<li>Availability: Data must be served correctly</li>
	<li>Performance: Data is arriving in timely manner</li>
  </ol>
  <p>Reliability is a crucial aspect of distributed systems, where the failure of any individual machine should not disrupt the overall functioning of the system. Consider a scenario like a large electronic commerce store such as Amazon. Here, it's essential that user transactions proceed seamlessly without being affected by machine failures.<br/>
     For example, when a user adds an item to their shopping cart, the expectation is that this action remains intact even if the machine handling the transaction fails. In other words, the system should not lose the user's shopping cart data due to a machine failure.<br/>
	 Achieving this level of reliability involves redundancy, both in terms of software components and data. In the case of the shopping cart example, if the server responsible for managing a user's cart fails, another server should seamlessly take over, ensuring that the user's data is preserved.<br/>
	 However, ensuring such redundancy comes with a cost. Maintaining duplicate software components and data replication requires additional resources and infrastructure. Yet, it's a necessary investment to ensure that the system remains resilient and can continue to operate without interruption, even in the face of failures. Ultimately, a reliable distributed system strives to eliminate any single points of failure to deliver consistent and uninterrupted service to its users.
  </p>
<h1>Good Reads</h1>
  <ul>
    <li><a href="https://www.youtube.com/watch?v=IhGWOaD5BYQ&ab_channel=USENIX">Scaling Reliability at Dropbox</a></li>
  </ul>
  
  
  