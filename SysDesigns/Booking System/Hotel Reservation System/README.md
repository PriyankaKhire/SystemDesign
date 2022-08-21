<h1>Hotel reservation system</h1>
  <h2>Requirements</h2>
    <h3>Functional Requirements</h3>
      <ul>
        <li>Assume we are building website for hotel chain that has 5000 hotels and 1million rooms.</li>
        <li>Customers pay in full when they make the reservation</li>
        <li>Customers are able to cancel their reservation</li>
        <li>We need to allow 10% over booking of the hotel rooms</li>
        <li>Price of the hotel room is different every day</li>
      </ul>
    <h3>Non-functional Requirements</h3>
      <ul>
        <li>During peak season, there might be several customers who might be trying to book the room at the same time. Our system should have high concurrency.</li>
        <li>Latency could be moderate. It is ok for the server to take some time to process the reservation.</li>
      </ul>
  <h2>Capacity Estimations</h2>
    <ol>
      <li><b>Write Operations:</b> We have 1 million rooms in total, let us <i>assume</i> that around 70% of the rooms are booked and average stay is 3 days.</li>
      <ol>
        <li>Total rooms reserved: 1 million * 70% = 700,000</li>
        <li>For 3 days we have 700,000 reservations, per day we have 700,000/3 ~= 240,000</li>
        <li>Write operations per hour: 240,000/24 ~= 10,000</li>
        <li>Write operations per minute: 10,000/60 ~= 167</li>
        <li>Write operations per second: 167/60 ~= 3</li>
      </ol>
      <li><b>Read Operations/Database Queries per second</b>: The following are different types of read operations from the DB</li>
      <ol>
        <li>View Hotel rooms for given dates </br><img src="img/View Rooms.png" height="300px"></li>
        <li>Get room details </br><img src="img/Room Details.png" height="300px"></li>
        <li>Reserve the room: we already know this is 3 per second from above calculations. For "view hotel room" and "get room details" we can make up some numbers or ask interviewer.</br><img src="img/Reserve Room.png" height="300px"></li>
      </ol>
    </ol>
  <h2>API Design</h2>
    <p>We will design APIs REST-style</p>
    <h3>Hotel Related</h3>
      <p><i>/v1/hotels/ID</i></p>
      <table>
        <tr><th>API</th><th>HTTP Request</th><th>Input</th><th>Output</th><th>Details</th></tr>
        <tr><td>GET /v1/hotels/ID</td><td>GET</td><td>Hotel Id</td><td>Hotel information in json format</td><td>Get detailed information about a hotel.</td></tr>
        <tr><td>POST /v1/hotels</td><td>POST</td><td>Json data provided in body of the POST message, containing hotel information like name, address etc.</td><td>Success if hotel was added successfully, else failure</td><td>Add a new hotel.</td></tr>
        <tr><td>PUT /v1/hotels/ID</td><td>PUT</td><td>ID of the hotel and json body containing the information you want to update</td><td>Success if the hotel information was updated successfully else failure</td><td>Update hotel information</td></tr>
        <tr><td>DELETE /v1/hotels/ID</td><td>DELETE</td><td>ID of the hotel</td><td>Success if the hotel is deleted successfully else failure</td><td>Delete a hotel.</td></tr>
      </table>
    <h3>Rool Related</h3>
      <p><i>/v1/hotels/ID/rooms/ID</i></p>
      <table>
        <tr><th>API</th><th>HTTP Request</th><th>Input</th><th>Output</th><th>Details</th></tr>
        <tr><td>GET /v1/hotels/ID/rooms/ID</td><td>GET</td><td>Room ID</td><td>Room information in json format</td><td>Get detailed room information</td></tr>
        <tr><td>POST /v1/hotels/ID/rooms</td><td>POST</td><td>Json data provided in body of the POST message, containing room information like room number, type etc.</td><td>Success if a room was added successfully, else failure.</td><td>Add a new room.</td></tr>
        <tr><td>PUT /v1/hotels/ID/rooms/ID</td><td>PUT</td><td>ID of the room and json body containing the information you want to update</td><td>Success if the room information was updated successfully else failure</td><td>Updated room information</td></tr>
        <tr><td>DELETE /v1/hotels/ID/rooms/ID</td><td>DELETE</td><td>ID of the room</td><td>Success if the room is deleted successfully else failure.</td><td>Delete a room.</td></tr>
      </table>
    <h3>Reservation Related</h3>
      <p><i>/v1/hotels/reservations/ID</i></p>
      <table>
        <tr><th>API</th><th>HTTP Request</th><th>Input</th><th>Output</th><th>Details</th></tr>
        <tr><td>GET /v1/reservations</td><td>GET</td><td>Json data provided in the body, for example: User Id, page offset(number of reservations per page)</td><td>Json data getting the past reservation history of the user.</td><td>Get reservation history of the user.</td></tr>
        <tr><td>GET /v1/reservations/ID</td><td>GET</td><td>User Id, reservation Id</td><td>Information about the reservation in json format</td><td>Get detailed information about a reservation.</td></tr>
        <tr><td>POST /v1/reservations</td><td>POST</td><td>Following Json data in the body of the message: </br>{</br>&nbsp;&nbsp;&nbsp;&nbsp;"startDate": "2022-08-14", </br>&nbsp;&nbsp;&nbsp;&nbsp;"endDate": "2022-08-25",</br>&nbsp;&nbsp;&nbsp;&nbsp;"hotelID": "12345",</br>&nbsp;&nbsp;&nbsp;&nbsp;"roomID": "50",</br>&nbsp;&nbsp;&nbsp;&nbsp;"userId": "1056",</br>&nbsp;&nbsp;&nbsp;&nbsp;"roomType": "King-Bed"</br>}</td><td>Success if the reservation was successful else failure</td><td>Make a new reservation</td></tr>
        <tr><td>PUT /v1/reservations/ID</td><td>PUT</td><td>ID of the reservation and json body containing the information you want to update</td><td>Success if the reservation was updated successfully else failure</td><td>Update a reservation</td></tr>
        <tr><td>DELETE /v1/reservations/ID</td><td>DELETE</td><td>ID of the reservation</td><td>Success if the reservation is deleted successfully else failure</td><td>Cancel a reservation</td></tr>
      </table>
  <h2>Data Model</h2>
    <p>Before choosing on how to model our data, let's take a look at what data we are interested in storing</p>
    <ol>
      <li><b>Hotel Related</b></li>
      <ul>
        <li>Hotel Id</li>
        <li>Hotel name</li>
        <li>Hotel address</li>
        <li>Number of rooms</li>
      </ul>
      <li><b>Room Related</b></li>
      <ul>
        <li>Room ID</li>
        <li>Room type (example: smoking, non-smoking)</li>
        <li>Room number</li>
        <li>Room is_available</li>
        <li>Room details</li>
      </ul>
      <li><b>Reservation Related</b></li>
      <ul>
        <li>Reservation ID</li>
        <li>Hotel ID</li>
        <li>Room ID</li>
        <li>User ID</li>
        <li>Start Date</li>
        <li>End Date</li>
      </ul>
      <li>User Related</li>
      <ul>
        <li>User ID</li>
        <li>Name</li>
        <li>Email</li>
      </ul>
    </ol>
    <h3>Which DB to use?</h3>
      <p>There are more users who view rooms and their details as opposed to booking them. So we know that this is a <b>Read Heavy</b> system.</p>
      <h4>Relational DB</h4>
        <ul>
          <li>Relational DBs work well with read-heavy systems because their fetch times are faster. You can also index a few fields to fetch them faster.</li>
          <li>They are ACID compliant. (Atomicity, consistency, isolation and durability) We need this property of DB in payment heavy systems. Because they help with problems such as double booking etc.</li>
          <li>The above listed data has clear relationship which can be modeled well in relational DB.</li>
        </ul>
      <h4>Non-relational DB</h4>
        <ul>
          <li>Many new non-relational DBs are ACID compliant such as Cassandra.</li>
        </ul>
    <h3>Schema</h3>
      <table>
        <tr><th colspan="2">Hotel Related</th></tr>
        <tr>
          <td>
            <table>
              <tr><th>Hotel</th></tr>
              <tr><td><b>id [Primary Key]</b></td></tr>
              <tr><td>name</td></tr>
              <tr><td>address</td></tr>
              <tr><td>number of rooms</td></tr>
            </table>
          </td>
          <td>
            <table>
              <tr><th>Room</th></tr>
              <tr><td><b>id [Primary Key]</b></td></tr>
              <tr><td>type id [Can add index on this]</td></tr>
              <tr><td>number</td></tr>
              <tr><td>is_available</td></tr>
              <tr><td><i>Hotel ID [Foreign Key]</i></td></tr>
            </table>
          </td>
        </tr>
      </table>
      <table>
        <tr><th colspan="3">Reservation Related</th></tr>
        <tr>
          <td>
            <table>
              <tr><th>Reservation</th></tr>
              <tr><td><b>id [Primary Key]</b></td></tr>
              <tr><td><i>Hotel ID [Foreign Key]</i></td></tr>
              <tr><td><i>Room ID [Foreign Key]</i></td></tr>
              <tr><td><i>User ID [Foreign Key]</i></td></tr>
              <tr><td>Start Date</td></tr>
              <tr><td>End Date</td></tr>
              <tr><td>Status [Pending, Paid, Refunded, Canceled, Rejected]</td></tr>
            </table>
          </td>
          <td>
            <table>
              <tr><th>Room Inventory</th></tr>
              <tr><td><i>Hotel ID [Foreign Key]</i></td></tr>
              <tr><td>Room Type ID</td></tr>
              <tr><td>Date</td></tr>
              <tr><td>Total available</td></tr>
            </table>
          </td>
          <td>
            <table>
              <tr><th>Room Rate</th></tr>
              <tr><td><i>Hotel ID [Foreign Key]</i></td></tr>
              <tr><td>Room type ID</td></tr>
              <tr><td>Date</td></tr>
              <tr><td>Rate</td></tr>
            </table>
          </td>
        </tr>
      </table>
      <table>
        <tr><th>User</th></tr>
        <tr><td><b>id [Primary Key]</b></td></tr>
        <tr><td>Name</td></tr>
        <tr><td>Email</td></tr>
      </table>
    <h3>Data Storage</h3>
      <p>We could have done this section in capacity estimations, but having some more context over data schema and what database to use helps.</p>
      <p>Let us calculate data storage for independent tables. This will help us in identifying how many instances of DB we need, how much space we need, what sharding scheme to use etc.</p>
      <ol>
        <li>Hotel Related</li>
        <ul>
          <li><b>Hotel Table:</b> Total 5000 hotels, so 5000 rows.</li>
          <li><b>Room Table:</b> Total 1million rooms, so those many rows. However, this table will be modified quiet frequently for the <i>is_available</i> field.</li>
        </ul>
        <li>Reservation Related</li>
        <ul>
          <li><b>Reservation Table:</b> From the capacity estimations we know that we have 240,000 reservations a day, so per year we have approximately 88 million reservations. Plus some reservations might be cancelled and we also need to account for 10% overflow booking, so we can assume there might be about 100 million rows in this table per year.</li>
          <li><b>Room Inventory Table:</b> Since we store room data per day, we will at least have 365 rows in this table per year.</li>
          <li><b>Room Rates Table:</b> The rates can change dynamically every day, so we can have 365 rows in this table per year.</li>
        </ul>
        <li>User: This table can have multiple entries, we can assume a number here for calculations, however remember there might also be guest users, who might not register. The size of this table will still not be as large.</li>
      </ol>
      <p>So, from above it is clear that Reservation table requires the most storage. 100Million rows per year is not a lot of storage, so we can get away with using just <b>single instance of DB</b>. However, single instance means single point of failure, to avoid this we can use replication schemes like master slave, where we read from slaves and write to master.</p>
      <p>Now for some reason the reservation table does grow a lot and we need to use sharding, then we can user <i>hotel id</i> as sharding key. (but what if one hotel is more popular, even then the max reservations per year would be 365 * number of rooms, which is not a big number and we can allocate a shard for it)</p>
  <h2>Avoiding Double Booking</h2>
  
  <h2>Good Reads</h2>
    <ul>
      <li><a href="https://medium.com/airbnb-engineering/avoiding-double-payments-in-a-distributed-payments-system-2981f6b070bb">Avoiding Double Payments in a Distributed Payments System</a></li>
    </ul>