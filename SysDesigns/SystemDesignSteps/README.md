<h1>How to approch System design</h1>

<h2>Goals</h2>
  <h3>Functional Design Goals</h3>
	  <ol>
		<li>Just what exactly am I designing here ?</li>
		<li>Who are the users (example: organization, university etc.)</li>
		<li>How will the users use this system ?</li>
		<li>Scenarios that will not be covered</li>
		<li>Assumptions made</li>
		<li><b>What are the input and outputs of the system ? </b></li>
	  </ol>

> Try to focus on just top 3 requirements.
> 
> Having a long list of requirements will hurt you more than it will help you, just focus on what matters.

  <h3>Non-Functional Design Goals</h3>
	  <ul>
		<li>Latency and Throughput requirements</li>
		<li>Consistency vs Availability 
		  <ul>
			<li>Weak/strong/eventual => consistency</li>
			<li>Fail-over/replication => availability</li>
		  </ul>
		</li>
	  </ul>

<h2>Scope</h2>
  <p>
    Am I designing this system for: <br/>
    <ul>
      <li> 1 User </li>
      <li> Multiple users </li>
      <li> Billions of users </li>
    </ul>
    <br/>
    Should I discuss end to end experience or just the API ? <br/>
  </p>

<h2>Capacity estimation</h2>
  <ul>
    <li>How big of a system am I actually designing ?</li>
    <li>How much data would be produced ?</li>
    <li><b>Throughput: </b>How many read write requests per second ?</li>
    <li>Read vs Write ratio (is the system read heavy or write heavy) 
      <ul>
        <li>Volume of Read data</li>
        <li>Volume of Write data</li>
      </ul>
    </li>
    <li><b>Latency</b> expected from the system (for read and write queries)</li>
    <li><b>Storage estimates:</b> What am I storing, and how many different databases will I be using.</li>
    <li><b>Memory estimates:</b>
      <ul>
        <li>If we are using a cache, what is the kind of data we want to store in cache</li>
        <li>How much RAM and how many machines do we need for us to achieve this ?</li>
        <li>Amount of data you want to store in disk/ssd</li>
      </ul>
    </li>
  </ul>

  <ul>
<content>So let's say you have 500Million monthly active users</content></br>
<content> That is 16Million users per day -> 600Thousand users per hour -> 10 Thousand users per min -> 166 users per second (These calculations may not be accurate, we are just estimating)</content></br>
<content>But this is a perfect world scenario, no application in real life is perfect and no scenario is perfect either</content></br>
<content>In real life traffic is unevenly distributed, so for example for an app like facebook, most people like to come home and check it. That means traffic probably increases after 5:00PM</content></br>
<content>So the question you should be instead asking is: </content></br>
<content>where are these users ?</content></br>
<content>Are these users distributed evenly geographically ? Because different countries will give you different traffic patterns</content></br>
<content>How much percent of these monthly active users are actually active daily ?</content></br></br>
<content> Let's say our above calculations were correct, that there would be 10Thousand active users per min in worst case senario</content></br>
<content>And we have written a server that can handle roughly about 1000 people per min.</content></br>
<content>That means we'd need about 10 servers to handle the load at given time</content></br>
<content> So you have taken a big problem of 500Million active users and broken it down </content></br>
</ul>
  On basis of all this, you can scale the system</br>
  Also, don't forget about the overall consumer impact of your system.</br>
  Like, you are designing a system, but will the consumer benefit from your design ? because if they don't they won't use the system</br>
<br>

>  Many guides you've read will suggest doing back-of-the-envelope calculations at this stage. We believe this is often unnecessary. Instead, perform calculations only if they will directly influence your design. In most scenarios, you're dealing with a large, distributed system – and it's reasonable to assume as much. Many candidates will calculate storage, DAU, and QPS, only to conclude, "ok, so it's a lot. Got it." As interviewers, we gain nothing from this except that you can perform basic arithmetic.
> 
>  Our suggestion is to explain to the interviewer that you would like to skip on estimations upfront and that you will do math while designing when/if necessary. When would it be necessary? Imagine you are designing a TopK system for trending topics in FB posts. You would want to estimate the number of topics you would expect to see, as this will influence whether you can use a single instance of a data structure like a min-heap or if you need to shard it across multiple instances, which will have a big impact on your design.

<h2>High Level diagram</h2>
<p>
Draw stuff... </br>
Start with high level block diagram.</br>
</p>

<h2>High Level Design</h2>
  <ol>
    <li>Design the classes</li>
    <li>API
    <ol>
      <li>Design what your APIs look like ?
        <ol>
         <li>Input</li>
         <li>Output</li>
        </ol>
      </li>
      <li>APIs for Read and Write Scenario</li>
    </ol>
    </li>
    <li>Write some code, what algorithms will you use ?</li>
    <li>High level design for Read/Write scenario</li>
  </ol>

  <h3>API Design</h3>
    **RESTful API**: The standard communication protocol of the internet. Uses HTTP verbs (GET, POST, PUT, DELETE) to perform CRUD operations on resources.<br>
    **For example**: For Twitter the REST api would look like this
    
```yaml
    POST /v1/tweet/create
    body: {
      "text": string
    }

    GET /v1/tweet/:tweetId -> Tweet

    POST /v1/follow/:userId

    GET /v1/feed -> Tweet[]
```
> Notice how there is no **UserId** in the POST /v1/tweet/create endpoint? This is because we will get the id of the user initiating the request from the authentication token in the request header. Putting sensitive information like authentication tokens in the request body is a security risk.

<h2>Define a data model</h2>
<p>
Ok, so we designed classes, but what is the primary key ? <br/>
How exaclty am I storing them ? <br/>
What database am I going to use ? SQL vs NoSQL ? <br/>
</p>

<h2>Detailed component design</h2>
<p>
Pick a component they are interested in and go deep there. </br>
</p>

<h2>Bottleneck of the system designed so far </h2>
<p>
Before proceeding to scale the system, we look at the bottlenecks of the system designed so far and discuss that with the interviewer.
</p>

<h2>Scale the system</h2>
<p><b> Remember: You NEED to have a working system before you can scale it. </b></p>
<ul>
  <li>Scale the individual components
  <ul>
    <li>Availability</li>
    <li>Consistency</li>
  </ul>
  </li>
  <li>Think about the following components, how they would fit in and how it would help
  <ol>
    <li>Load balancers (be careful though, they are expensive)</li>
    <li>Cache
      <ol>
        <li>Client side Vs Server side</li>
        <li>Eviction Policies: LRU, LFU, FIFO</li>
        <li>Caching Patterns: Cache aside, Write through, Write ahead, Refresh ahead</li>
      </ol>
    </li>
    <li>CDN [Push vs Pull]</li>
    <li>Asynchronism
    <ol>
      <li>Message queues</li>
      <li>Task queues</li>
      <li>Back pressure ??</li>
    </ol>
    </li>
    <li>Communication
      <ol>
        <li>TCP</li>
        <li>UDP</li>
        <li>REST</li>
        <li>RPC</li>
      </ol>
    </li>
  </ol>
  </li>
  <li>Database
    <ul>
      <li>Relational
      <ol>
        <li>Master-slave</li>
        <li>Master-master</li>
        <li>Sharding</li>
      </ol>
      </li>
      <li>Non-Relational
        <ol>
          <li>Key-Value</li>
          <li>Graph</li>
          <li>Wide-Column</li>
        </ol>
        Fast lookups databases:
        <ul>
          <li>RAM  [Bounded size] => Redis, Memcached</li>
          <li>AP [Unbounded size] => Cassandra, Voldemort, RIAK</li>
          <li>CP [Unbounded size] => HBase, MongoDB, Couchbase, DynamoDB</li>
        </ul>
      </li>
    </ul>
  </li>
</ul>
</br>

<h2>Tradeoffs</h2>
<p>
Don't forget to discuss tradeoffs of big systems</br>
Do this every time you bring up new things </br>
Identify bottlenecks </br>
</p>

<h2>Justify</h2>
<ul>
  <li>Throughput of each layer</li>
  <li>Latency caused between each layer</li>
  <li>Overall latency justification</li>
</ul>

<h2>Helpful Tips</h2>
Start with the simplest version of the problem and work your way up. For example, if you're asked to design a chat application, start with a 1:1 chat application and then work your way up to a group chat application.


<h2>Good Reads</h2>

- [Leet code system design template](https://leetcode.com/discuss/career/229177/My-System-Design-Template)
- [System design in a hurry](https://www.hellointerview.com/learn/system-design/in-a-hurry/delivery)
