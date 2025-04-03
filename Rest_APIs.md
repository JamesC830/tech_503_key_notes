# Rest APIs notes

## HTTP

**Hypertext Transfer Protocol**

![HTTP](https://www.ionos.co.uk/digitalguide/fileadmin/DigitalGuide/Screenshots_2020/diagram-of-http-communication-process.png)

- Foundational technology for rest APIs
- Lets 2 computures communicate with eachother

**Request responce**:
- ***Request***: User does something to request data from server
- ***Responce***: Server responds with requested data and the browser renders it

**Statelessness**: Each request is independant and isloated

Request example:
![req](https://www.tutorialspoint.com/http/images/http-message-request.jpg)

### Cookies
Data stored by the browser to remember information about the user

**Uses**: Identify user, session management, marketing, personalisation, tracking

### Status codes
- 1xx: Continue
- 2xx: OK (request successful)
- 3xx: Redirection
- 4xx: Error (Client error)
- 5xx: Server error

**HTTPS**: Communication is encrypted between client and serveer. The client has a key for decryption

## Rest APIs

**APIs** (Application programming interface): Mechanism which allows 2 programs  to communicate

**REST** (Representational State Transfer): Architectural style for designing network structures

**Constraints**
- Client-server achitecture (Client and server are seperate)
- Stateless
- Cacheability
- Layered system: Client shouldn't know what goes on behind interface
- Uniform interface

### CRUD

Create, Read, Update, Delete

Request methods: 
- POST: Creates new resource
- GET
- PUT: Replaces resource
- DELETE
- PATCH: Partially updates a resource

**Rest endpoint**: Correctly formatted URL

**API key**: Unique identifier used to authenticate a client accessing an API

**HATEOAS** (Hypertext As The Engine Of Application State): Client interacts with the API entierly through hypermedia (links)

