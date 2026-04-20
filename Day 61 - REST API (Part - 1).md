## API

- An API (Application Programming Interface) is a software interface that allows two applications to communicate with each other.
- APIs are essential not just for network automation, but for all kinds of applications.
- In SDN architecture, APIs are used to communicate between apps and the SDN controller (via the NBI), and between the SDN controller and the network devices (via the SBI).
- The NBI typically uses REST APIs.
- NETCONF and RESTCONF are popular southbound APIs.

![[Pasted image 20260404174927.png]]

## CRUD 

- CRUD (Create, Read, Update, Delete) refers to the operations we perform using REST APIs.

- Create operations are used to create new variables and set their initial values.  
	- ie. create variable “ip_address” and set the value to “10.1.1.1”.

- Read operations are used to retrieve the value of a variable.  
	- ie. what is the value of variable “ip_address”?

- Update operations are used to change the value of a variable.  
	- ie. change the value of variable “ip_address” to “10.2.3.4”.

- Delete operations are used to delete variables.  
	- ie. delete variable “ip_address”.

- HTTP uses verbs (aka. methods) that map to these CRUD operations.
- REST APIs typically use HTTP.

## HTTP Verbs

![[Pasted image 20260404175115.png]]

![[Pasted image 20260404175202.png]]

- When an HTTP client sends a request to an HTTP server, the HTTP header includes information like this:  
	- An HTTP Verb (ie. GET)  
	- A URI (Uniform Resource Identifier), indicating the resource it is trying to access.

### HTTP Request 

![[Pasted image 20260404175307.png]]

## HTTP Response 

- The server’s response will include a status code indicating if the request succeeded or failed, as well as other details.

- The first digit indicates the class of the response:  
	- 1xx informational – the request was received, continuing process  
	- 2xx successful – the request was successfully received, understood, and accepted  
	- 3xx redirection – further action needs to be taken in order to complete the request  
	- 4xx client error – the request contains bad syntax or cannot be fulfilled
	- 5xx server error - the server failed to fulfill an apparently valid request

![[Pasted image 20260404175610.png]]

- Here are some examples of each HTTP Response class:

- 1xx Informational  
	- 102 Processing indicates that the server has received the request and is processing it, but the response is not yet available.

- 2xx Successful  
	- 200 OK indicates that the request succeeded.  
	- 201 Created indicates that the request succeeded and a new resource was created (ie. in response to POST)

- 3xx Redirection  
	- 301 Moved Permanently indicates that the requested resource has been moved, and the server indicates its new location.

- 4xx Client Error  
	- 401 Valid credentials not provided.
	- 403 Unauthorized means the client must authenticate to get a response.  
	- 404 Not Found means the requested resource was not found.

- 5xx Server Error  
	- 500 Internal Server Error means the server encountered something unexpected that it doesn’t know how to handle.

## REST

- REST stands for Representational State Transfer.

- REST APIs are also known as REST-based APIs or RESTful APIs.  
	- REST isn’t a specific API. Instead, it describes a set of rules about how the API should work.

- The six constraints of RESTful architecture are:  
	- Uniform Interface  
	- Client-server  
	- Stateless  
	- Cacheable or non-cacheable  
	- Layered system  
	- Code-on-demand (optional)

- For applications to communicate over a network, networking protocols must be used to facilitate those communications.
	- For REST APIs HTTP(S) is the most common choice.

>Remember the CRUD actions, HTTP client request verbs, HTTP server response codes, and the basics characteristics of REST APIs.

## REST: Client-server

- REST APIs use a client-server architecture.
- The client uses API calls (HTTP requests) to access the resources on the server.

- The separation between the client and server means they can both change and evolve independently of each other.  
- When the client application changes or the server application changes, the interface between them must not break.

![[Pasted image 20260404180033.png]]

## REST: Stateless

- REST APIs exchanges are stateless.

- This means that each API exchange is a separate event, independent of all past exchanges between the client and server.  
	- The server does not store information about previous requests from the client to determine how it should respond to new requests.

- If authentication is required, this means that the client must authenticate with the server for each request it makes.
- TCP is an example of a stateful protocol.
- UDP is an example of a stateless protocol.

*Although REST APIs use HTTP, which uses TCP (stateful) as its Layer 4 protocol, HTTP and REST APIs themselves aren’t stateful. The functions of each layer are separate!*

# **Quiz**

![[Pasted image 20260404180701.png]]

![[Pasted image 20260404180713.png]]

![[Pasted image 20260404180724.png]]

![[Pasted image 20260404180740.png]]

![[Pasted image 20260404180758.png]]

![[Pasted image 20260404180842.png]]







