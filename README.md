<h1>Basics</h1>
  <h2>Network Protocols</h2>
    <p>Network protocols make it possible for any networked computers to talk to each other</p>
    <h3>Networking Models</h3>
      <ol>
        <li>TCP/IP Model</li>
        <li>OSI Model</li>
      </ol>
    <h3>Networking Protocols</h3>
      <ol>
        <li><b>Internet Protocol:</b> It is the key protocol that allows computers in different physical networks to communicate with each other</li>
        <li><b>Transport Control Protocol (TCP):</b> It works by first establishing a connection between the client and server, and then transferring data. It builds on IP to add guarantees that data messages are delivered reliably and, in order</li>
        <li><b>User Datagram Protocol (UDP):</b> It works at the same layer as TCP, but has no guarantees about data delivery or ordering. So if the application needs faster data transfer and doesn’t require a confirmed connection we can use UDP</li>
        <li><b>Hypertext Transport Protocol (HTTP):</b> It allows applications to view and modify data over the network </li>
      </ol>
  <h2>Polling</h2>
    <p>Polling is the process where the client (web browser), makes an HTTP request to the server, which sends back the appropriate response.</p>
    <h3>Short Polling</h3>
      <ol>
        <li>The client sends a HTTP request to the server requesting information.</li>
        <li>The server, if it has any information sends it back.</li>
        <li>The client then waits for some time and repeats the above two steps again.</li>
      </ol>
      <p>The problem with Polling is that the client has to keep asking the server for any new data. As a result, a lot of responses are empty, creating HTTP overhead.</p>
      <img src="img/PollingSSEAndWebSockets/ShortPolling.png">
    <h3>Long Polling</h3>
      <ol>
        <li>The client opens a TCP tunnel and sends a HTTP request to the server requesting information.</li>
        <li>The server waits until there is new information available and then sends the response back.</li>
        <li>After receiving the information the client immediately makes another request and repeats the above two steps.</li>
        <li>If the server doesn’t have any new data and the long polling request times out, then the client has to make another request.</li>
      </ol>
      <p>The server has to handle the case where it gets new information to send, but the client hasn’t sent a new request yet.</p>
      <p>The HTTP long polling mechanism can be applied to either persistent or non-persistent HTTP connections. The use of persistent HTTP connections will avoid the additional overhead of establishing a new TCP/IP connection [TCP] for every long poll request.</p>
      <img src="img/PollingSSEAndWebSockets/LongPolling.png">
    
  <h2>Server Sent Events (SSE)</h2>  
    <p>In this process, the server establishes a one way, long term connection with the client. Only the server is allowed to push data to the client. If the client wants to send data to the server, it needs to use another technology/protocol to do so.</p>
    <p>This way the client can send data to the server, without having to re-establish a connection every time.</p>
    <ol>
      <li>The client requests for a new SSE connection. </li>
      <li>The server registers the new SSE connection.</li>
      <li>The server begins pushing new data to the client.</li>
      <li>Either sides are allowed to close the connection.</li>
    </ol>
    <p>The main benefit of SSEs is it provides an efficient one directional data stream where the client and server don’t need to constantly reestablish the connection.</p>
    <img src="img/PollingSSEAndWebSockets/SSE.png">
    
  <h2>WebSockets</h2>
    <p>It is a two-way message passing protocol based on TCP. WebSockets are faster for data transmission than HTTP.</p>
    <ol>
      <li>The client establishes a WebSocket connection with the server, through a process known as the WebSocket handshake.</li>
      <li>The messages are transmitted in both directions over port 443.</li>
      <li>Either side can close the connection.</li>
    </ol>
    <p>The main advantage of WebSockets is speed: the client and server don’t have to find and reestablish their connection with each other every time a message is sent.</p>
    <p>TCP ensures that the messages will always arrive in order.</p>
    <p>The main downside of WebSockets is it takes a good amount of initial developer work to implement. </p>
    <img src="img/PollingSSEAndWebSockets/WebSockets.png">
    <p>An example where WebSockets is really useful is multiplayer online gaming, </p>  
    
<h1>System Design</h1>
