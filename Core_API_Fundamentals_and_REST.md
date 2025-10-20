#  **Core API Fundamentals & REST**

Think of an API as a waiter in a restaurant.

- You (the Client) are the customer.

- The Kitchen (the Server) has the food 
(the Data) and the logic to prepare it.

- The Waiter (the API) takes your request, 
communicates it to the kitchen, and brings back your response.

## 1. HTTP Methods (GET, POST, PUT, PATCH, DELETE)

**Core Concept:** These are the __verbs__ or __actions__ you can perform on a resource. 
They tell the server what kind of operation you want to do.

|Method |Real-World Analogy                                                                                              |Technical Use Case & Example                                                                                                          |
|-------|----------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
|GET    |**Looking at a menu.** You're asking for information without changing anything.                                 |**Retrieving data.** (GET /api/users) - Gets a list of all users.                                                                     |
|POST   |**Placing an order.** You're giving the kitchen new information that creates something new (your order ticket). |**Creating a new resource.** (POST /api/users) - Creates a new user. (Sends user data in the request body).                           |
|PUT    |**Giving the chef a completely new recipe card for a dish.** You are replacing the entire thing.                |**Replacing a resource entirely.** (PUT /api/users/123) - Updates all the data for user #123.                                         |
|PATCH  |**Telling the chef "please make this dish less spicy."** You're only updating a specific part.                  |**Partially updating a resource.** (PATCH /api/users/123) - Updates only the email for user #123 - (Sends {"email": "new@mail.com"}). |
|DELETE |**Canceling an order.** You're asking for something to be removed.                                              |**Deleting a resource.** (DELETE /api/users/123) - Deletes user #123.                                                                 |

## 2. HTTP Status Codes (2xx, 3xx, 4xx, 5xx)

**Core Concept:** These are short numeric codes the server sends back to tell you the __result of your request.__ 
It's the waiter saying "Coming right up!" or "Sorry, we're out of that."

|Code Range           |Real-World Analogy                               |Common Examples & Meaning                                                                                                                                                                     |
|---------------------|-------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|**2xx Success**      |**"Everything is good!"**                        |200 -> OK (Request succeeded). / 201 -> Created (Something was created, often after a POST).                                                                                                  |
|**3xx Redirection**  |**"What you're looking for is now in Aisle 5."** |301 -> Moved Permanently (The resource has a new permanent URL).                                                                                                                              |
|**4xx Client Error** |**"You made a mistake."**                        |400 -> Bad Request (Your request was malformed). / 401 -> Unauthorized (You need to log in). / 403 -> Forbidden (You don't have permission). / 404 -> Not Found (The resource doesn't exist). |
|**5xx Server Error** |**"The kitchen is on fire."** The server failed. |500 -> Internal Server Error (A generic server-side error). / 503 -> Service Unavailable (The server is overloaded or down for maintenance).                                                  |

## 3. HTTP Headers

**Core Concept:** These are metadata (extra information) sent with both the request and the response. 
They control how the data is handled.

**Real-World Example:** The information on a package's shipping label: "Fragile," "This End Up," "Signature Required."

**Technical Examples:**
- Request Headers:
  - Authorization: Bearer (token) - Proves your identity.
  - Content-Type: application/json - Tells the server what format the data you're sending is in.
- Response Headers:
  - Content-Type: application/json - Tells your client what format the returned data is in.
  - Cache-Control: max-age=3600 - Tells your client it can store this response for 1 hour.

## 4. Content Negotiation & Media Types

**Core Concept:** A way for the client and server to agree on the **data format** to be used. 
The client says what it can understand, and the server responds with one of those formats.

**Real-World Example:** You (the client) walk into a shop and say, "I speak English or French." 
The shopkeeper (the server) then responds in English.

**Technical Example:**
- A client sends a request with the header: Accept: application/json, application/xml.
- This means: "I prefer JSON, but I can handle XML if you have to."
- The server, if it can, will send back the data as JSON and include Content-Type: application/json in the response header.

## 5. RESTful Design Principles & Constraints

**Core Concept:** REST is a set of architectural rules for building scalable and reliable web services. 
The core idea is to treat everything as a **resource**.

- **Key Principles:**

  - **Stateless:** The server doesn't remember anything about the client between requests. Every request must contain all the information needed to understand it. (Like giving the waiter your table number every time you call them over).
  - **Client-Server:** A separation of concerns. The client handles the UI/UX, the server handles data storage and logic.
  - **Uniform Interface:** This is where using standard HTTP methods (GET, POST, etc.) and clear URIs comes from. It makes the system predictable.

## 6. Resource-Oriented Architecture & URI Design

**Core Concept:** You design your API around **nouns (resources)**, not **verbs (actions)**. 
Your URIs (Uniform Resource Identifiers) should reflect the hierarchy of these resources.

**Real-World Example:** Think of a library's organization: /floors/3/sections/Sci-Fi/books/Dune

**Technical Examples (Good vs. Bad):**

- **Good (Resource-Oriented):**
  - GET /users - Get all users.
  - GET /users/123 - Get user with ID 123.
  - GET /users/123/orders - Get all orders for user 123.
  - GET /users/123/orders/456 - Get order #456 for user 123.

- **Bad (Action-Oriented - Avoid this):**
  - GET /getAllUsers
  - POST /createUser
  - GET /getUserOrders

## 7. API Contracts & Contract-First Design

**Core Concept:** An **API Contract** is a formal agreement that specifies exactly how the client and server will communicate. 
It defines the endpoints, request/response formats, data types, and errors.

**Contract-First Design** means you **write this contract BEFORE you write any code.**

**Real-World Example:** Building a house. You create the blueprints (the contract) first. 
All the electricians, plumbers, and carpenters follow the same blueprint. 
This prevents the plumber from putting a pipe where the electrician needs to run a wire.

**Technical Example:** You use a specification like **OpenAPI (Swagger)** to write a YAML or 
JSON file that defines your entire API. This file can then be used to 
automatically generate documentation, server code stubs, and client libraries.

## 8. Idempotency & Safe Operations

**Core Concept:**

- **Safe:** An operation (like GET) that doesn't change the state of the server. 
You can call it a million times without any side effects.
- **Idempotent:** An operation that can be called multiple times without producing 
a different result beyond the initial request.

**Real-World Example:**

- **Safe:** Reading a news article online. Reading it 10 times doesn't change the article.
- **Idempotent:** Toggling a light switch from "OFF" to "ON." Pressing the "ON" button 10 times has the same result as pressing it once—the light is ON.

**Technical Summary:**

- **Safe Methods:** GET, HEAD
- **Idempotent Methods:** GET, HEAD, PUT, DELETE

## 9. Designing APIs for Scalability & Maintainability

**Core Concept:** Building your API so it can handle growth (more users) and be easily changed and understood by other developers.

**How to achieve it:**

- **Use Clear & Consistent Naming:** (e.g., always use plural nouns: /users, not /user).
- **Version Your API:** Put a version in the URI (/api/v1/users) or a header. This allows you to make breaking changes without breaking existing clients.
- **Use Pagination, Filtering, Sorting:** For endpoints that return lists, don't return everything. Use GET /api/users?page=2&limit=50&sort=name to avoid overloading the server and client.
- **Provide Meaningful Error Messages:** Don't just return 400 Bad Request. Return a body with a helpful message: {"error": "The 'email' field is required."}
- **Keep it Simple (KISS):** Start with simple endpoints. Don't over-engineer for hypothetical future needs.