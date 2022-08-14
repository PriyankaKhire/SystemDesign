<h1>Movie ticket booking system / Hotel reservation system</h1>
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
        <tr><td>POST /v1/reservations</td><td>POST</td><td>Following Json data in the body of the message: </br>{</br>&nbsp;&nbsp;&nbsp;&nbsp;"startDate": "2022-08-14", </br>&nbsp;&nbsp;&nbsp;&nbsp;"endDate": "2022-08-25",</br>&nbsp;&nbsp;&nbsp;&nbsp;"hotelID": "12345",</br>&nbsp;&nbsp;&nbsp;&nbsp;"roomID": "50",</br>&nbsp;&nbsp;&nbsp;&nbsp;"userId": "1056",</br>}</td><td>Success if the reservation was successful else failure</td><td>Make a new reservation</td></tr>
        <tr><td>PUT /v1/reservations/ID</td><td>PUT</td><td>ID of the reservation and json body containing the information you want to update</td><td>Success if the reservation was updated successfully else failure</td><td>Update a reservation</td></tr>
        <tr><td>DELETE /v1/reservations/ID</td><td>DELETE</td><td>ID of the reservation</td><td>Success if the reservation is deleted successfully else failure</td><td>Cancel a reservation</td></tr>
      </table>
  
  
  <h2>Good Reads</h2>
    <ul>
      <li><a href="https://medium.com/airbnb-engineering/avoiding-double-payments-in-a-distributed-payments-system-2981f6b070bb">Avoiding Double Payments in a Distributed Payments System</a></li>
    </ul>