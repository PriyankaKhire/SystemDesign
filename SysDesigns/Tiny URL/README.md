<h1>Tiny URL</h1>
  <h2>Requirements</h2>
    <ol>
      <li>You are given a long URL, your service creates a shorter url.</li>
      <li>When you click this short URL, your service redirects to the original longer URL.</li>
      <li><b>Traffic volume</b>: 100 Million URLs are generated per day</li>
      <li>The shortened URL can be a combination of numbers (0-9) and alphabets (a-z, A-Z)</li>
      <li>The system needs to be highly available. scalable and fault tolerant.</li>
      <li>The application should have minimal latency</li>
    </ol>
  <h2>Capacity Estimations</h2>
    <ol>
      <li><b>Write Operations</b>: 100 Million URLs are generated per day.</li>
      <ol>
        <li>Write operations per hour: 100 million / 24 = 4166666.66667</li>
        <li>Write Operations per minute: 100 million / (24*60) = 69444.4444444</li>
        <li>Write Operations per second: 100 million / (24*60*60) ~= 1160</li>
      </ol>
      <li><b>Read operations</b>: Assume there are 10 reads per write</li>
      <ol>
        <li>Read operations per second: 1160 * 10 ~= 11600</li>
      </ol>
      <li><b>Storage</b>: Assume the URL shortener service will run for 10 years</li>
      <ol>
        <li>We need to store 100 million * 365 * 10 ~= 365 billion URLs</li>
        <li>If average URL length is 100 characters then for 10 years we'd need: 365 billion * 100 bytes ~= 36.5TB of storage</li>
      </ol>
      <li><b>Bandwidth</b>: Let's say the size of each long URL is 500bytes at max.</li>
      <ol>
        <li>Write bandwidth per second: 1160*500bytes ~= 580KB/s</li>
        <li>Read bandwidth per second: 11600*500bytes ~= 5.8 MB/s</li>
      </ol>
    </ol>
  <h2>APIs</h2>
    <p>We will design APIs REST-style</p>
    <h3>URL shortening</h3>
      <p>To create new short URL, the client sends a POST request. This request contains the original long URL</p>
      <p><i>POST api/v1/data/shorten</i></p>
      <ul>
        <li>Input: {longURL: long URL string, userName: username of the user creating tiny URL, authenticationToken: auth token to authenticate user.}</li>
          <ul><li>We can even rate limit the number of requests sent per user by using their auth key to prevent resource consumption attacks.</li></ul>
        <li>Output: Short URL</li>
      </ul>
    <h3>URL Redirecting</h3>
      <p>The client sends a GET request to get redirected to the original long URL after inputting the Short URL</p>
      <p><i>GET api/v1/shortUrl</i></p>
      <ul><li>Output: Returns long URL for HTTP redirection</li></ul>
      <h4>Types of Redirects</h4>
        <ol>
          <li><b>301 Redirect</b>: This response tells the browser that the URL is "permanently moved". Thus the browser will cache the response, so all the future request for this URL will not be sent to the server. So the browser will end up doing the job for you. This response is used if you want to reduce the server load.</li>
          <li><b>302 Redirect</b>: This response tells the browser that the URL is "temporarily moved". So all the future redirect requests will still be sent to the URL shortening server, from there you get the long url and get redirected. This response code is mainly used if you want to count the clicks or where the requests are coming from, basically the analytics.</li>
        </ol>
  <h2>Data Model</h2>
    <p>Our service is read heavy.</p>
    <p>Instead of storing everything in hash table or key value store, a good approach is to store the <i>short URL: Long URL</i> mapping in relational DB.</p>
    <table>
      <tr><th>URL</th></tr>
      <tr><td><b>id [Primary Key]</b></td></tr>
      <tr><td>short URL</td></tr>
      <tr><td>long URL</td></tr>
    </table>
    <h3>Which DB to use ?</h3>
      <p>We mentioned above that using a relational DB is a good idea because our system is read heavy. Also, if we want to add user data and their login information then relational DB is still a good idea. You can even consider putting user sessions data in a different DB independent of this DB to make architecture more stateless. </br> Even using a non-relational DB is not a bad idea, specially if you want to scale further horizontally. </br> So really both databases can work here.</p>
    <h3>Data Partitioning and Replication</h3>
      <p>Now we can't just store all the billion's of URLs on one instance of DB, we need to partition it.</p>
      <ul>
        <li><b>Range Based Partitioning</b>: Store all hash keys beginning with letter A in one shard, B on another shard and so on</li>
        <ul><li>This can cause unbalenced shards, one shard might have more data than the other.</li></ul>
        <li><b>Hash Based Partitioning</b>: Take the short URL put it into hash function and let it decide which shard to put it on, based on the hash value it has generated.</li>
        <ul><li>This will add a lot of overhead, not to mention another server to manage, plus added latency of lookups etc.</li></ul>
      </ul>
  <h2>Hash Function</h2>
    <p>The hash function is the one that maps the long URL to short URL.</p>
    <img src="img/Basic Hash Function.png">
    <h3>Calculating the hash value length</h3>
      <p>The short URL consists of [0-9, a-z, A-Z] which is (10 digits + 26 small letters + 26 capital letters = 62 characters) and we need to store total 365 billion urls. (remember capacity estimations) </br> So to get the smallest length of hash value we need <i>n</i> such that 62^n >= 365 billion</p>
      <table>
        <tr><th>n</th><th>Max number of URLs</th></tr>
        <tr><td>1</td><td>62^1 = 62</td></tr>
        <tr><td>2</td><td>62^2 = 3844</td></tr>
        <tr><td>3</td><td>62^3 = 238328</td></tr>
        <tr><td>4</td><td>62^4 = 14776336</td></tr>
        <tr><td>5</td><td>62^5 ~= 916 Million</td></tr>
        <tr><td>6</td><td>62^6 ~= 56 Billion</td></tr>
        <tr><td>7</td><td>62^7 ~= 3.5 Trillion</td></tr>
      </table>
      <p>So it's safe to assume that our hash value length is 7</p>
    <h3>Types of hash functions</h3>
      <h4>Hash + Collision Resolution</h4>
        <p>We can use some of the well known functions like <b>CRC32, MD5, SHA-1</b>. However, these hash functions may not produce result that has the length of 7. (Also, the length 7 is for 10 years of storage, we may want more storage or less storage so our length number may change)</p>
        <h5>Solution 1</h5>        
          <p>So how do we solve this problem of length ? We can just take the first 7 characters of the generated hash value and then check in DB if there is an entry present with that value. If there is then we append a predefined string to our original long URL and generate a new hash value. Do this until we get a unique value that is not present in DB.</p>
          <img src="img/Hash Collision Resolution.png">
          <p>The problem with this approach is that we need to hit the DB every time we have to check if the hash value exists or not.</p>
        <h5>Solution 2</h5>
          <p>Another approach to solving the length problem is we can append to the long URL, to the following:
            <ul>
              <li><i>user ID</i></li>
              <li><i>increasing sequence number</i> (we can have the DB generate this number). </li>
              <li><i>epoch time stamp + thread id</i> (thread id only if we are worried for multiple requests coming in at the same time.)</li>
            </ul> 
            We then pass this new long URL through the hash function and take the first 7 characters, this will make every long URL unique.
          </p>
          <img src="img/Hash Collision Resolution pt2.png">
          <p>Problem with this approach is can the ever increasing sequence of number overflow ? Or if we are appending userId then what happens if user is not signed in or it's a guest account.</p>
      <h4>Base 62 conversion</h4>
        <p>Below image represents the base 62 table</p>
        <img src="img/Base 62 Table.png">
        <p>The idea with this approach is we can have our relational database generate unique incremental id of length 7 (we can even make this as our Primary key) then take this number and convert it in to base 62. </br> Let us take an exaple to understand this idea.</p>
        <table>
          <tr><th>Number</th><th>Remainder</th><th>Representation in base 62</th></tr>
          <tr><td>11157 / 62 = 179</td><td>59</td><td>x</td></tr>
          <tr><td>179 / 62 = 2</td><td>55</td><td>t</td></tr>
          <tr><td>2 / 62 = 0</td><td>2</td><td>2</td></tr>
        </table>
        <p>So the answer is <b>2tx</b></p>
        <p>One major flaw of this approach is the generated hash value length is not fixed. For example the base 62 value of 1234567 is 5ban</p>
      <h4>Generating keys offline</h4>
        <p>We can have a standalone service that generates keys offline and stores them in DB. We can even have a special dedicated DB for these newly generated keys. </br> So, when we want to shorten any URL we can just take one of these pre-generated keys and use it.</p>
        <img src="img/KGS.png">
        <p>The problem with this approach is what if multiple servers want to access key generating service at the same time ? We can solve this by making 2 tables in keys DB, one table for used keys and another for unused keys. As soon as "offline keys generating service" gives away a key, it can move it to used keys table. Also our key generating server needs to make sure it doesn't give same key to multiple servers, it can do so by putting lock on the data structure holding those keys.</p>
  <h2>High Level Design Diagram</h2>
    <p>Shorten the URL</p>
    <img src="img/High Level Diagram.png">
    <p>Retrieve the long URL from Short URL</p>
    <img src="img/High Level Diagram Redirect.png">
      
  
