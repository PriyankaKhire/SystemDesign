# API Design Concepts
## API Requirements
When designing an API, it's essential to consider both functional and non-functional requirements to ensure it is effective, efficient, and useful. 
### Functional 
- **Resource Access:** APIs should be able to perform basic operations on data resources, typically defined as CRUD operations: Create, Read, Update, Delete.
- **Authentication and Authorization:** Determine which users are allowed to access the API, ensuring that only authorized users can perform certain actions.
- **Data Input Validation:** The API must validate data sent to it
- **Error Handling:** The API should gracefully handle errors and give clear error messages to help users understand what went wrong.

### Non-Functional

- **Performance**: The API should respond quickly to requests. Performance metrics like response time and throughput need to be specified.
- **Scalability**: The API should be able to handle increases in load, whether from more data, more users, or both, without degrading performance.
- **Security**: The API must secure user data and protect against threats such as SQL injection, Cross-Site Scripting (XSS), and data breaches.
- **Reliability**: The API should be dependable, operating correctly and consistently as expected.  
- **Availability**: The API should be up and running at all times as defined by uptime metrics (e.g., 99.9% uptime).   
- **Maintainability**: The API should be easy to update and maintain, with clear documentation and standards.  
- **Usability**: The API should be easy to use for developers, with well-documented methods, parameters, and examples.

<img src="img/API Requirements.png">

## Characteristics of a good API Design
- **Separation of API Specification and Implementation**
  - Keep the API's external behaviors separate from the internal execution.
  - Allows for changes and improvements without altering the interface.

- **Concurrency**
  - Determines how many API calls can be active simultaneously.
  - Ensures computing resources are available for all users.

- **Dynamic Rate-Limiting**
  - Limits access to the API within a specified timeframe to prevent overload.

- **Security**
  - Implements strong authentication and authorization protocols.
  - Controls access to the API and its functionalities.

- **Error Warnings and Handling**
  - Manages errors effectively to reduce user frustration and debugging efforts.

- **Architectural Styles**
  - The API can adopt different architectural styles based on its requirements.

- **Minimal but Comprehensive**
  - The API should be simple yet complete and cohesive in its functionality.

- **Stateless or State-Bearing**
  - Operations may be stateless (not storing data) or state-bearing (storing data).
  - Idempotency is desired, ensuring the same operations produce the same results.

- **User Adoption**
  - Widely adopted APIs often have a supportive community that aids in ongoing improvement.

- **Fault Tolerance**
  - Well-designed APIs include mechanisms to continue operation, even when some components fail.

- **Performance Measurement**
  - Essential to have monitoring systems to track performance and detect issues early.
  
## SLI (Service Level Indicator)
Is a metric used to measure the performance of a given service against a defined standard.
Some common SLIs:
- **Request Latency** How long it takes to return a response to a request.
- **Error Rate** The number of failed requests divided by the number of total requests in a specified period.
- **System Throughput** The rate at which a system processes and completes tasks or the amount of work that a system can handle over a given period.
- **Availability** The fraction of time when the service is usable.
- **Durability** The likelihood that data will be retained over time.

## SLO (Service Level Objective)
An SLO defines the level of service expected by setting clear goals for performance and reliability that the service provider aims to meet within a given time frame. These objectives are crucial in managing and meeting customer expectations for the quality of the service provided.

## SLA (Service Level Agreement)
A service level agreement (SLA) is a contract that outlines the service’s terms and conditions. It also provides the service’s availability and performance, as well as other features that the customer can anticipate from the service. The SLA also defines the metrics and methods for measuring the service, such as uptime, response time, throughput, and errors. It also establishes the remedies or consequences if the service fails to meet the agreed-upon objectives.

### Difference between SLA and SLO
An **SLA** (Service Level Agreement) and an **SLO** (Service Level Objective) are both used to measure the quality of service provided by a company, but they are used in different ways. Here’s a simple explanation of how they differ:

- **SLA (Service Level Agreement)**:
  - This is a formal contract between a service provider and a customer.
  - It includes all the details of the services provided, including the standards the service needs to meet, what happens if the standards aren't met (like penalties or compensation), and the responsibilities of both the provider and the customer.
  - Think of it as a complete rulebook for what the customer can expect from the service and what happens if those expectations are not met.

- **SLO (Service Level Objective)**:
  - This is a specific goal or target within that larger agreement. It focuses on one aspect of the service, like how fast a server should respond to requests or how often it should be up and running without problems.
  - It's a more precise measure that helps both the service provider and the customer understand if the service is meeting the expected quality.
  - Think of it as a specific goal in the rulebook that the service provider aims to achieve to keep their customers happy and maintain quality service.

In essence, the SLA is the whole agreement that includes all the details of the service relationship, while the SLO is a specific target or goal within that agreement aimed at ensuring good service quality.