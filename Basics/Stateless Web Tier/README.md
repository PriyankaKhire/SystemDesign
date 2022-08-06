<h1>Stateless Web Tier</h1>
  <p>If we want to scale the web server horizontally, we need to move the state (example: sessions data) out of the web server. When each web server in the cluster can access the state data from a DB, it's called stateless web tier</p>
  <p>Below is an example of stateful server architecture</p>
  <img src="img/Stateful Architecture.png">
  <p>The issue with this type of architecture is that every request from the same user must be rerouted to the same server. And we can do this using load balencers but this just adds extra overhead. Plus adding and removing servers becomes much more difficult.</p>
  </br>
  <p>Below is the diagram of stateless architecture.</p>
  <img src="img/Stateless Architecture.png">
  <p>In stateless architecture the HTTP requests from any use can be navigated to any server, because the server then fetches the sessions data from the shared sessions DB.</p>
  <p>A stateless system is more scalable. </p>
