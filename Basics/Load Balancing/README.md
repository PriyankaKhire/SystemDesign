<h1>Load Balancing</h1>
  <p> It helps to distribute traffic evenly across multiple servers.
    If a server is not available they stop sending traffic to that server.
    LB sits between a client and server. It balances out the requests across multiple servers, thus reducing individual server load. <br/>
  </p>
  <p>Generally we can LB in 3 places.
    <ul>
      <li>Between user and web server</li>
      <li>Between web server and application server</li>
      <li>Between application server and DB</li>
    </ul>
  </p>
  <h2>Cost</h2>
  <p>Load Balancers are generally VERY expensive, starting from $20k. Make sure to talk about cost in mind while adding LBs.
  </p>
  <h2>Single point of failure</h2>
    <p>We can add a second LB thus forming a cluster.
      The passive one always monitors the health of the active one.
      In case the active LB fails the passive LB can take its place.
    </p>
  <h2>Load Balancing Algorithms (Haven't mentioned all)</h2>
    <ul>
      <li>Consistent hashing</li>
      <li><b>Round Robin</b>: Cycles through list of servers and sends traffic to next server</li>
      <li><b>Least Connection Method</b>: Sends traffic to server with fewer connections.</li>
    </ul>
  <h2>Authenticating users using LB</h2>
    <p>You can configure an Application Load Balancer to securely authenticate users as they access your applications. This enables you to offload the work of authenticating users to your load balancer so that your applications can focus on their business logic.</p>
  <h2>Good Reads</h2>
    <ul>
      <li><a href="https://www.youtube.com/watch?v=-W9F__D3oY4&t=1260s">Harvard Lecture by David Malan - Load Balancing and Caching</a> ⭐</li>
      <li><a href="https://docs.aws.amazon.com/elasticloadbalancing/latest/application/listener-authenticate-users.html">Authenticate users using an Application Load Balancer</a></li>
      <li><a href="https://www.f5.com/services/resources/glossary/load-balancer">Load Balancer by F5 networks</a></li>
      <li><a href="https://aws.amazon.com/elasticloadbalancing/?whats-new-cards-elb.sort-by=item.additionalFields.postDateTime&whats-new-cards-elb.sort-order=desc">Elastic Load Balancers by AWS</a></li>
    </ul>
  
  
  
  