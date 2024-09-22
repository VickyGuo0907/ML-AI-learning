# Basic System Design and Software Architecture Concepts

## System Design Basics Questions

### 1. Difference between JWT, OAuth and SAML? 

Here's a breakdown of the differences between **JWT**, **OAuth**, and **SAML**:

1. **JWT (JSON Web Token)**
	- **Purpose**: JWT is a token format used for securely transmitting information between parties as a JSON object. It can be used for authentication and information exchange.
	- **Structure**: A JWT consists of three parts: header, payload, and signature. The payload can include claims (e.g., user ID, roles) and is encoded in Base64.
	- **Usage**: Commonly used for stateless authentication in web applications (e.g., APIs) where the server can verify the token's signature to authenticate requests.
	- **State**: Stateless; once issued, the server doesn't need to maintain session state.
	- **Transport**: Typically sent in HTTP headers (e.g., Authorization: “Bearer token”).
2. **OAuth (Open Authorization)**
	- **Purpose**: OAuth is an authorization framework that allows third-party applications to access a user’s data without exposing their credentials. It is often used for delegating access.
	- **Flow**: Involves multiple roles: resource owner, client, authorization server, and resource server. OAuth uses access tokens to grant permission to the client.
	- **Usage**: Commonly used for authorizing access to APIs (e.g., logging in with Google or Facebook).
	- **State**: Can be stateful or stateless, depending on implementation. Access tokens can be short-lived, and refresh tokens can be used to obtain new access tokens.
3. **SAML (Security Assertion Markup Language)**
	- **Purpose**: SAML is an XML-based framework used for Single Sign-On (SSO) across different domains. It allows users to authenticate once and gain access to multiple applications.
	- **Structure**: Uses XML to encode the assertions that contain user authentication and attribute information.
	- **Usage**: Commonly used in enterprise environments for SSO between service providers and identity providers.
	- **Transport**: Typically involves redirects between the user's browser and the identity provider, using browser-based profiles.
	- **State**: Often stateful, as it involves server-side sessions and assertions.

**Summary**
- **JWT** is a token format mainly used for authentication.
- **OAuth** is an authorization framework that enables access delegation using tokens.
- **SAML** is primarily used for Single Sign-On, enabling authentication across different domains.

Each serves a specific purpose in identity and access management, with different mechanisms and use cases.

### 2. What's different between Reverse Proxy and Forward Proxy?

[Detail Materials](https://dev.to/somadevtoo/difference-between-forward-proxy-and-reverse-proxy-in-system-design-54g5)

1. **Forward Proxy**
	- **Definition**: A forward proxy sits **between a client and the internet**. It acts on behalf of the client, making requests to external servers and forwarding the responses back to the client.
	- **Primary Role**: 
	  - **Client-side proxy**: The forward proxy is used by the client to access the internet.
	  - It hides the client’s IP address from the destination server.
	- **Use Cases**:
	  - **Content Filtering**: Restricts or monitors users’ access to certain websites (e.g., in schools or offices).
	  - **Caching**: Speeds up repeated requests by caching content for multiple clients.
	  - **Anonymity**: Allows clients to hide their real IP addresses (e.g., in VPNs or Tor).
	  - **Bypassing Geo-blocks**: Access content that is restricted by location.
	- **Example**: A user in a company network uses a forward proxy to access an external website. The website only sees the proxy’s IP, not the user’s.

2. **Reverse Proxy**
	- **Definition**: A reverse proxy sits **between the internet and a server** (or group of servers). It handles requests from clients, forwards them to the appropriate server, and then sends the server’s response back to the client.
	- **Primary Role**:
	  - **Server-side proxy**: The reverse proxy is used by the server to manage incoming traffic from the internet.
	  - It hides the backend server’s IP address from the client.
	- **Use Cases**:
	  - **Load Balancing**: Distributes incoming traffic across multiple servers to optimize performance and avoid overload.
	  - **Caching**: Caches responses from servers to speed up future requests and reduce server load.
	  - **SSL Termination**: Handles SSL/TLS encryption and decryption, offloading the work from the backend servers.
	  - **Security**: Protects backend servers by filtering malicious traffic and hiding their identities.
	- **Example**: A client requests a website, and the reverse proxy forwards the request to one of several web servers. The client is unaware of the backend server's details, seeing only the reverse proxy.

### Key Differences
- **Direction**: 
  - A **forward proxy** works on behalf of the client, shielding the client from the external network.
  - A **reverse proxy** works on behalf of the server, shielding the server from the external client.
  
- **Visibility**:
  - A **forward proxy** hides the client from the server.
  - A **reverse proxy** hides the server(s) from the client.

- **Use Cases**:
  - **Forward Proxy**: Used for client-side purposes like anonymity, caching, and content filtering.
  - **Reverse Proxy**: Used for server-side purposes like load balancing, security, and traffic management.

In short, forward proxies primarily help clients access external resources, while reverse proxies manage and optimize traffic for servers.

### 3. Horizontal scaling and vertical scaling?
[Detail Materials](https://dev.to/somadevtoo/horizontal-scaling-vs-vertical-scaling-in-system-design-3n09)

Here's a breakdown of the differences between **Horizontal Scaling** and **Vertical Scaling**:

 1. **Horizontal Scaling (Scaling Out)**

	- **Definition**: Horizontal scaling involves adding more machines (or nodes) to a system to distribute the load across multiple resources. This is also known as "scaling out."
	  
	- **How It Works**: 
	  - Instead of upgrading a single machine, you add more servers or instances to the system. Each new machine or instance shares the workload, helping to increase capacity.
	  - Load balancers are often used to distribute traffic across these multiple instances.
	
	- **Advantages**:
	  - **Improved Fault Tolerance**: If one machine fails, others can continue handling the load.
	  - **Easier to Scale**: You can scale by simply adding more machines, especially in cloud environments (e.g., AWS, Azure).
	  - **Better for Distributed Systems**: Works well with microservices and cloud-native applications.
	  
	- **Challenges**:
	  - **Complexity**: Managing multiple servers introduces complexity, requiring strategies like load balancing, data consistency, and distributed management.
	  - **Data Replication**: Ensuring consistency across distributed nodes can be challenging, especially for stateful applications.
	
	- **Example**: A web application that serves increasing numbers of users adds more servers to handle the growing traffic, with each server handling a portion of the workload.

2. **Vertical Scaling (Scaling Up)**

	- **Definition**: Vertical scaling involves increasing the capacity of a single machine by adding more resources (e.g., CPU, memory, or storage). This is known as "scaling up."
	
	- **How It Works**:
	  - The server itself is upgraded by adding more RAM, CPU, storage, or faster components.
	  - The application continues to run on the same server but with more powerful resources.
	
	- **Advantages**:
	  - **Simpler Management**: There's only one server to manage, making it easier from an operational perspective.
	  - **No Changes to Architecture**: The underlying architecture remains the same, and no additional load balancing or distribution strategies are needed.
	  - **Immediate Performance Boost**: More resources directly improve performance on the single machine.
	
	- **Challenges**:
	  - **Limits to Scaling**: There's a physical limit to how much you can scale up a single machine. At some point, you'll hit resource limits.
	  - **Single Point of Failure**: If the server goes down, the entire application could become unavailable, unless additional redundancy measures are in place.
	  - **Cost**: Upgrading a single machine to handle larger loads can become very expensive, especially for high-end hardware.

- **Example**: A database server is experiencing high load, so more RAM and CPUs are added to improve performance without adding more machines.

 **Key Differences**

| **Aspect**               | **Horizontal Scaling (Scaling Out)**                | **Vertical Scaling (Scaling Up)**                |
|--------------------------|-----------------------------------------------------|--------------------------------------------------|
| **Method**                | Adding more machines (nodes/instances)              | Upgrading the capacity of a single machine        |
| **Capacity Increase**     | Distributes load across multiple machines           | Increases the power of a single machine           |
| **Fault Tolerance**       | High (more machines = more redundancy)              | Low (a single machine failure can take down the system) |
| **Complexity**            | More complex (load balancing, data replication)     | Simpler (no need for load balancing)              |
| **Scalability**           | Virtually unlimited (add more machines as needed)   | Limited by physical hardware constraints          |
| **Cost**                  | Typically cheaper at scale (cloud environments)     | Can become expensive as hardware needs grow       |

**Summary**

- **Horizontal Scaling** (Scaling Out) is adding more machines to distribute the load. It’s great for high availability, fault tolerance, and handling very large-scale systems.
- **Vertical Scaling** (Scaling Up) is increasing the capacity of a single machine. It’s simpler to implement but limited by hardware constraints and presents a single point of failure.

Both approaches have their use cases, and many systems use a combination of both to meet their needs.

### 4. what's different btween Microservices and Monolithic architecture?

Here’s a comparison between **Microservices** and **Monolithic** architecture:

1. **Monolithic Architecture**
	- **Definition**: In a monolithic architecture, the entire application is built as a single, unified unit. All components (UI, business logic, database) are part of one codebase and are deployed as a single application.
	
	- **Structure**: 
	  - The application is a single executable or process where all features and functions are tightly coupled.
	  - Components like user interfaces, business logic, and data access layers are part of the same system.
	
	- **Advantages**:
	  - **Simplicity**: Easier to develop, test, and deploy because everything is in one codebase.
	  - **Performance**: Communication between components is in-memory, making interactions faster.
	  - **Development Speed**: For smaller applications, development is faster since everything is centralized.
	  
	- **Challenges**:
	  - **Scalability**: Scaling requires scaling the entire application, even if only one part needs additional resources.
	  - **Maintenance**: As the application grows, maintaining and updating the code becomes difficult due to tight coupling.
	  - **Deployment**: Any small change requires redeploying the entire application, which can slow down release cycles.
	  - **Flexibility**: Harder to adopt new technologies because the whole system is built using a single stack.
	
	- **Use Case**: Suitable for small or simple applications where scalability, deployment, and maintenance complexities are not critical concerns.

2. **Microservices Architecture**
	- **Definition**: In a microservices architecture, the application is composed of small, independent services that communicate with each other, typically over a network. Each service focuses on a specific business capability and can be developed, deployed, and scaled independently.
	
	- **Structure**:
	  - Each microservice is a separate, autonomous component, typically running as an individual process.
	  - Microservices communicate over lightweight protocols (e.g., HTTP/REST, gRPC, or message queues).
	
	- **Advantages**:
	  - **Scalability**: Services can be scaled independently based on their resource needs, allowing more efficient use of infrastructure.
	  - **Resilience**: Failure of one service doesn't necessarily bring down the entire system. Services are isolated, which improves fault tolerance.
	  - **Flexibility**: Teams can choose different technologies, languages, and databases for different services, which provides technological freedom.
	  - **Faster Deployments**: Small, independent services can be deployed and updated without redeploying the entire application, allowing for more frequent releases.
	  - **Team Autonomy**: Teams can work on different services in parallel, improving development speed and ownership.
	
	- **Challenges**:
	  - **Complexity**: Managing a distributed system introduces complexity, especially around inter-service communication, data consistency, and failure handling.
	  - **Latency**: Communication between services over a network introduces latency compared to in-memory communication in monolithic systems.
	  - **Deployment and Monitoring**: Requires sophisticated tooling for CI/CD pipelines, monitoring, and troubleshooting across multiple services.
	  - **Data Management**: Ensuring consistency across services can be challenging since each service might manage its own database.
	
	- **Use Case**: Suitable for large, complex, and scalable applications where flexibility, independent scaling, and frequent updates are required.

**Key Differences**

| **Aspect**                    | **Monolithic Architecture**                       | **Microservices Architecture**                             |
| ----------------------------- | ------------------------------------------------- | ---------------------------------------------------------- |
| **Structure**                 | Single codebase and process                       | Multiple, independent services                             |
| **Scalability**               | Scales as a whole (vertically)                    | Scales individual services (horizontally)                  |
| **Deployment**                | Single deployment unit                            | Each service is deployed independently                     |
| **Communication**             | In-memory communication between components        | Network-based communication (e.g., HTTP, messaging)        |
| **Fault Tolerance**           | Failure in one part may affect the entire system  | Failure in one service does not affect the whole app       |
| **Development Speed**         | Fast for small apps but slows as the system grows | Faster with independent teams for different services       |
| **Technological Flexibility** | Limited to a single tech stack                    | Different services can use different tech stacks           |
| **Data Management**           | Shared database for the entire application        | Each service can manage its own database                   |
| **Testing**                   | Easier for smaller apps, harder for larger apps   | More complex testing, requiring integration testing        |
| **Complexity**                | Lower for small apps, higher for large apps       | Higher due to distributed nature and service orchestration |

**Summary**

- **Monolithic Architecture** is simpler and easier to manage for smaller applications but can become hard to scale and maintain as the application grows.
- **Microservices Architecture** offers flexibility, scalability, and resilience but introduces complexity, requiring more sophisticated infrastructure and management strategies.

Many companies start with a monolithic architecture for simplicity and transition to microservices as their application and teams grow.

### 5. Difference between Vertical and horizontal partitioning ?

**Vertical Partitioning** and **Horizontal Partitioning** are techniques used in databases to improve performance, scalability, and manageability by dividing data across tables or databases. Here’s a breakdown of both:

1. **Vertical Partitioning**
	- **Definition**: Vertical partitioning involves splitting a table into multiple tables or partitions based on columns. Different groups of columns are stored in separate partitions, but each partition contains the same rows (identical primary keys).
	
	- **How It Works**:
	  - The original table’s columns are divided into smaller, more focused tables.
	  - Each partition typically contains a subset of columns, along with the primary key or identifier to link the partitions.
	
	- **Advantages**:
	  - **Performance**: Reduces the number of columns scanned for certain queries, which can improve query performance.
	  - **Security**: Sensitive data (e.g., personal info) can be stored separately, making access control and encryption easier.
	  - **Manageability**: Different partitions can be placed on different physical storage based on usage patterns or storage requirements.
	
	- **Use Cases**:
	  - When some columns in a table are accessed much more frequently than others (e.g., an "orders" table where the customer’s shipping address is rarely accessed but the order details are accessed often).
	  - When certain fields are sensitive and need to be handled with additional security layers.
	
	- **Example**: 
	  - A table called `Users` might be split into two tables:
	    - `User_Basic_Info`: (User_ID, First_Name, Last_Name, Email)
	    - `User_Details`: (User_ID, Address, Date_Of_Birth, Phone_Number)
	  - Both tables share the same `User_ID` for joining them when necessary.

2. **Horizontal Partitioning (Sharding)**
	- **Definition**: Horizontal partitioning involves splitting a table into multiple tables or partitions based on rows. Different sets of rows are stored in different partitions, each partition containing all columns but only a subset of rows.
	
	- **How It Works**:
	  - The original table is divided into several smaller tables where each partition contains the same set of columns, but each partition has different rows.
	  - Rows are typically divided based on a specific key or range (e.g., customer IDs, geographic regions).
	
	- **Advantages**:
	  - **Scalability**: Horizontal partitioning distributes rows across multiple databases or servers, which allows for better scaling across systems.
	  - **Performance**: Each partition handles fewer rows, improving query performance for queries targeting specific partitions.
	  - **Load Distribution**: Workload is distributed, reducing the load on individual partitions.
	
	- **Use Cases**:
	  - When a table becomes too large to handle efficiently or when the volume of reads and writes is extremely high.
	  - When the data is naturally partitionable, such as splitting user data by region or time periods.
	
	- **Example**:
	  - A `Customers` table is split based on geographic regions:
	    - `Customers_Region_A`: Contains customer data from Region A.
	    - `Customers_Region_B`: Contains customer data from Region B.
	  - Each table has the same columns (Customer_ID, Name, Address, etc.), but different rows based on the region.

**Key Differences**

| **Aspect**                 | **Vertical Partitioning**                         | **Horizontal Partitioning**                    |
|----------------------------|---------------------------------------------------|------------------------------------------------|
| **Basis of Partitioning**   | Columns (splits by grouping columns)              | Rows (splits by grouping rows)                 |
| **Structure**               | Each partition holds a subset of columns          | Each partition holds a subset of rows          |
| **When to Use**             | When certain columns are accessed or stored separately | When tables become too large and need to be distributed |
| **Performance Benefits**    | Improves query performance by reducing column scanning | Improves performance by reducing the number of rows in each partition |
| **Complexity**              | Easier to manage, but may require more joins for queries | More complex to implement (sharding, routing queries) |
| **Scalability**             | Improves column-specific access patterns          | Great for distributing large datasets across multiple nodes or servers |
| **Use Case**                | Storing less-frequently accessed columns in separate partitions | Distributing large user bases or geographic data |

**Summary:**
	- **Vertical Partitioning** splits a table based on columns, improving performance for specific queries and offering easier management of certain columns, especially in terms of security and access control.
	- **Horizontal Partitioning** (sharding) splits the table based on rows, which is highly beneficial for scaling and distributing large datasets across multiple databases or servers.

Both techniques can be used together in some scenarios to optimize different aspects of data storage and access.

### 6. What's different between API Gateway vs Load Balancer?

[Detail Materials](https://medium.com/javarevisited/difference-between-api-gateway-and-load-balancer-in-microservices-8c8b552a024)

Here's a breakdown of the differences between an **API Gateway** and a **Load Balancer**:

1. **API Gateway**

	- **Definition**: An API Gateway is a server that acts as an entry point for managing and routing API requests from clients to multiple back-end services or microservices. It handles request routing, composition, protocol translation, authentication, rate limiting, and more.
	
	- **Key Functions**:
		- **Request Routing**: Directs API requests to the appropriate microservice or backend.
		- **Authentication & Authorization**: Manages user authentication, security tokens, and access control.
		- **Rate Limiting**: Controls the number of requests clients can make within a certain time period to prevent overloading.
		- **Protocol Translation**: Translates between different protocols (e.g., HTTP to gRPC, WebSockets).
		- **Response Aggregation**: Combines responses from multiple services into a single response before sending it back to the client.
		- **Caching**: Can cache responses to reduce load on services and speed up response times.
		- **Logging & Monitoring**: Tracks requests and performance metrics for insights and troubleshooting.

	- **Use Cases**:
		- **Microservices Architectures**: Manages communication between clients and multiple microservices.
		- **Security**: Adds authentication, SSL termination, and threat detection at a single point before requests hit backend services.
		- **API Management**: Provides additional tools for API lifecycle management, such as versioning and rate limiting.

	- **Example**: 
		- In an e-commerce application with multiple microservices (user management, payment, product catalog), an API Gateway routes incoming API requests to the appropriate service (e.g., `/products`, `/checkout`) and handles authentication and rate limiting.

	

2. **Load Balancer**

	- **Definition**: A Load Balancer distributes incoming network or application traffic across multiple servers (or instances) to ensure no single server is overwhelmed, improving availability and reliability.
	
	- **Key Functions**:
		- **Traffic Distribution**: Spreads incoming traffic across multiple servers to ensure no server gets overloaded.
		- **Health Checks**: Monitors server health and redirects traffic away from failed or slow servers.
		- **Session Persistence**: Ensures that requests from the same user are routed to the same server for a consistent session experience.
		- **SSL Termination**: Can handle SSL decryption and offload that work from the backend servers.
		- **Scaling**: Can dynamically add or remove servers based on the traffic load to ensure performance is maintained.
		- **Failover**: Automatically redirects traffic to healthy instances when others fail.

	- **Use Cases**:
		- **Web Applications**: Distributes requests to multiple web servers, improving performance and reliability.
		- **Redundancy & Failover**: Ensures the application is still accessible even if some servers go down.
		- **High Availability**: Helps achieve 24/7 uptime by balancing traffic and recovering from failures.

	- **Example**: 
		- In a traditional web app with multiple identical servers running behind a load balancer, the load balancer distributes user requests to different servers based on server load, ensuring efficient use of resources.


**Key Differences**

| **Aspect**                    | **API Gateway**                                           | **Load Balancer**                                    |
|-------------------------------|----------------------------------------------------------|------------------------------------------------------|
| **Primary Purpose**            | Manages and routes **API** requests, handles authentication, rate limiting, and more | Distributes **network/application traffic** across servers |
| **Focus**                      | Focused on API management, security, and orchestration   | Focused on traffic distribution and server load management |
| **Routing Logic**              | Routes based on **API paths** (e.g., `/users`, `/orders`) | Routes based on server availability and load balancing algorithms |
| **Authentication**             | Supports **authentication** and authorization (e.g., OAuth, JWT) | Typically doesn't handle authentication; focus is on traffic routing |
| **Protocol Support**           | Can handle **multiple protocols** (e.g., HTTP, WebSockets, gRPC) | Typically supports **HTTP/HTTPS** and TCP/UDP traffic |
| **Advanced Features**          | Provides **rate limiting**, caching, API versioning, request aggregation | Provides **failover**, session persistence, and server health checks |
| **Use Case**                   | Ideal for **microservices** and managing complex API ecosystems | Ideal for **scaling web applications** or services by balancing traffic across servers |
| **Response Aggregation**       | Can combine responses from multiple services into one response | Does not aggregate responses; purely traffic distribution |
| **Security**                   | Handles **security policies**, SSL termination, and threat detection at the API level | Can offload **SSL termination**, but primarily focuses on traffic distribution |

---

**Summary:**

	- **API Gateway**: Provides comprehensive API management, including routing, authentication, rate limiting, protocol translation, and response aggregation. It is designed to handle **API traffic** and is particularly useful in **microservices** environments.
	
	- **Load Balancer**: Primarily responsible for distributing **network or web traffic** across multiple servers to balance the load and ensure high availability and fault tolerance. It is more about ensuring **scalability and performance**.

In short, an **API Gateway** focuses on managing and optimizing **API requests** while a **Load Balancer** focuses on **distributing traffic** across servers to balance load and ensure availability. Many modern architectures use both, with the load balancer sitting at the network level and the API gateway handling API-level concerns.