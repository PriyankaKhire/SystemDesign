# API Gateway
An API Gateway is a management tool that acts as a single point of entry for all the API calls from clients (like apps or web browsers) to services (like data storage or processing services). 
<img src="img/API Gateway.png">

API Gateway Acts as the middleman between clients and various backend services. It routes client requests to the appropriate service and can also handle other tasks like:
- **Authentication:** Checking if the client is allowed to access the service.
- **Rate Limiting:** Making sure a client doesn't send too many requests in a short period.
- **Caching:** Storing responses so the same request doesn't need to be processed by the backend every time.