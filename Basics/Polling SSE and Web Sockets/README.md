<h1>Polling</h1>
  <p>Polling is the process where the client (web browser), makes an HTTP request to the server, which sends back the appropriate response.</p>
  <h2>Short Polling</h2>
    <ol>
      <li>The client sends a HTTP request to the server requesting information.</li>
      <li>The server, if it has any information sends it back.</li>
      <li>The client then waits for some time and repeats the above two steps again.</li>
    </ol>
    <p>The problem with Polling is that the client has to keep asking the server for any new data. As a result, a lot of responses are empty, creating HTTP overhead.</p>
    <img src="img/ShortPolling.png"><br/>
    Example of short polling:<br/>
    <code>00:00:00 C-> Is the cake ready? </code><br/>
    <code>00:00:01 S-> No, wait. </code><br/>
    <code>00:00:01 C-> Is the cake ready? </code><br/>
    <code>00:00:02 S-> No, wait. </code><br/>
    <code>00:00:02 C-> Is the cake ready?  </code><br/>
    <code>00:00:03 S-> Yes. Have some lad. </code><br/>
    <code>00:00:03 C-> Is the other cake ready? .. </code><br/>
    
  <h2>Long Polling</h2>
    <ol>
      <li>The client opens a TCP tunnel and sends a HTTP request to the server requesting information.</li>
      <li>The server waits until there is new information available and then sends the response back.</li>
      <li>After receiving the information the client immediately makes another request and repeats the above two steps.</li>
      <li>If the server doesn’t have any new data and the long polling request times out, then the client has to make another request.</li>
    </ol>
    <p>The server has to handle the case where it gets new information to send, but the client hasn’t sent a new request yet.</p>
    <p>The HTTP long polling mechanism can be applied to either persistent or non-persistent HTTP connections. The use of persistent HTTP connections will avoid the additional overhead of establishing a new TCP/IP connection [TCP] for every long poll request.</p>
    <img src="img/LongPolling.png">  <br/>
    Example of long polling:<br/>
    <code>12:00 00:00:00 C-> Is the cake ready? </code><br/>
    <code>12:00 00:00:03 S-> Yes.Have some lad. </code><br/>
    <code>12:00 00:00:03 C-> Is the cake ready? </code><br/>
	
  <h2>Interval Polling and Adaptive Polling</h2>
    <h3>Interval Polling</h3>
	  <ol>
	    <li>The client sets a predetermined polling interval and sends a HTTP request to the server at fixed intervals.</li>
		<li>The server responds to each request, providing updates <i>if available</i> or indicating no updates.</li>
		<li>After receiving a response, the client evaluates factors such as <u>server response times or network conditions</u>.</li>
		<li>Based on this evaluation, the client dynamically adjusts the polling interval for subsequent requests.</li>
		<li>The client repeats the process by sending requests at the adjusted intervals and continues to dynamically adjust the polling interval based on ongoing evaluations.</li>
		<li></li>
	  </ol>
	  Example of Interval Polling: <br/>
	  <code>00:00:00 C-> Is the cake ready? </code><br/>
      <code>00:00:05 S-> No, wait. </code><br/>
	  <code>00:00:10 C-> Is the cake ready? </code><br/>
      <code>00:00:15 S-> No, wait. </code><br/>
	  <code>00:00:20 C-> Is the cake ready? </code><br/>
	  <code>00:00:25 S-> Yes. Have some lad. </code><br/>
	  <code>00:00:30 C-> Is the other cake ready? </code><br/>
	  <code>00:00:35 S-> No, wait. </code><br/>
	  <p>The client (C) repeatedly asks the server (S) if the cake is ready at fixed intervals (every 5 seconds). The client (C) continues to ask about the cake's readiness at the fixed intervals, regardless of the server's responses.</p>
	  
	<h3>Adaptive Polling</h3>
      <ol>
	    <li>The client sends a HTTP request to the server at a predetermined frequency, initially.</li>
		<li>After each response from the server, the client analyzes the content of the response to determine if there are any updates or changes.</li>
		<li>If updates are detected, the client may increase the polling frequency to receive updates more frequently.</li>
		<li>Conversely, if no updates are received or if updates are infrequent, the client may decrease the polling frequency to conserve resources and reduce unnecessary requests.</li>
		<li>The client continues to adjust the polling frequency dynamically based on the presence or absence of updates.</li>
	  </ol>
	  Example of Adaptive Polling: <br/>
	  <code>00:00:00 C-> Is the cake ready? </code><br/>
	  <code>00:00:01 S-> No, wait. </code><br/>
	  <code>00:00:05 C-> Is the cake ready? </code><br/>
	  <code>00:00:06 S-> Yes. Have some lad. </code><br/>
	  <code>00:00:09 C-> Is the other cake ready? </code><br/>
	  <code>00:00:10 S-> No, wait. </code><br/>
	  <p>When the client gets the response from the server that the cake is not ready it adjusts the frequency of polling.</p>
  
<h1>Server Sent Events (SSE)</h1>  
  <p>In this process, the server establishes a one way, long term connection with the client. Only the server is allowed to push data to the client. If the client wants to send data to the server, it needs to use another technology/protocol to do so.</p>
  <p>This way the client can send data to the server, without having to re-establish a connection every time.</p>
  <ol>
    <li>The client requests for a new SSE connection. </li>
    <li>The server registers the new SSE connection.</li>
    <li>The server begins pushing new data to the client.</li>
    <li>Either sides are allowed to close the connection.</li>
  </ol>
  <p>The main benefit of SSEs is it provides an efficient one directional data stream where the client and server don’t need to constantly reestablish the connection.</p>
  <img src="img/SSE.png"><br/>
  Example of SSE:<br/>
  <code>12:00 00:00:00 C-> Is the cake ready? </code><br/>
  <code>12:00 00:00:03 S-> Yes.Have some lad.  </code><br/>
  <code>12:00 00:00:05 S-> Here is more cake.  </code><br/>
  <code>12:00 00:00:08 S-> Here is some more cake.  </code><br/>

<h1>WebSockets</h1>
  <p>It is a two-way message passing protocol based on TCP. WebSockets are faster for data transmission than HTTP.</p>
  <ol>
    <li>The client establishes a WebSocket connection with the server, through a process known as the WebSocket handshake.</li>
    <li>The messages are transmitted in both directions over port 443.</li>
    <li>Either side can close the connection.</li>
  </ol>
  <p>The main advantage of WebSockets is speed: the client and server don’t have to find and reestablish their connection with each other every time a message is sent.</p>
  <p>TCP ensures that the messages will always arrive in order.</p>
  <p>The main downside of WebSockets is it takes a good amount of initial developer work to implement. </p>
  <img src="img/WebSockets.png"><br/>
  Example of Websockets:<br/>
  <code>00:00 00:00:00 C-> Is the cake ready? </code><br/>
  <code>00:00 00:00:03 S-> Yes.Have some lad.  </code><br/>
  <code>00:00 00:00:05 C-> I want more cake ? </code><br/>
  <code>00:00 00:00:05 S-> Here is some more cake.  </code><br/>
    
  <p>An example where WebSockets is really useful is multiplayer online gaming, </p>  
  