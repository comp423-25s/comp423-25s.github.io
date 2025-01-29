# Part 5. Paths, Parameters, Bodies, Headers, and Methods - Inputs of an HTTP Request

When working with RESTful APIs, there are multiple ways to provide input to a request, each serving a different purpose:

- **Methods** define the type of action the API request is performing.
- **Paths** define the structure of an API and uniquely identify resources.
- **Query Parameters** allow clients to filter, sort, or refine results without changing the resource's identity.
- **Bodies** are used to send complex data in requests, typically for creating or updating resources.
- **Headers** provide metadata about the request, such as authentication details or content type preferences.

Understanding these inputs is essential for designing REST APIs that are intuitive, efficient, and scalable.

## Introduction to REST

**REST (Representational State Transfer)** defines a set of architectural principles for designing web APIs that focus on resources and how to interact with them. A key characteristic of REST is that it's **stateless**—each request from client to server must contain all the information needed to understand and process the request. The server shouldn't need to remember anything about previous requests.

## Resources and Their Role in RESTful APIs

In RESTful APIs, the nouns in your API are **resources**. Each resource is uniquely identified by a **URL (Uniform Resource Locator)**, meaning every resource has its own address.

You may know these as web addresses, like:

- [https://csxl.unc.edu/api/organizations](https://csxl.unc.edu/api/organizations) → Returns a list of CS student organizations in JSON.
- [https://csxl.unc.edu/api/academics/section/term/25S](https://csxl.unc.edu/api/academics/section/term/25S) → Returns a list of CS classes offered in Spring 2025.

!!! warning "Could you easily see the structure of the JSON in the URLs above?"

    Most web browsers don't have nice, automatic formatting of JSON data. If you open the links above and it's not easy to see the structure of the data, we **strongly** recommend installing a web browser plugin to format JSON data. They are super handy when designing and building your own APIs!

    In Google Chrome, for example, we recommend [JSON Formatter](https://chromewebstore.google.com/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa). Other browsers have similar tools—find one with good reviews and many installations.

## Methods: Defining Actions

The **method** of an HTTP request tells the server what action is being performed on the resource. You previously read about the fundamental API methods in the last section, including **GET, POST, PUT, PATCH, and DELETE**. While we won't go into detail here, it's important to recognize that the method is an essential input to an HTTP request.


## Paths: Identifying and Routing Requests

The **path** of a URL is an opaque string of characters, but most API designers model it like a **logical hierarchy**, much like folders in a file system:

- `/api/users` → Might represent a collection of users.
- `/api/users/42` → Might represent a specific user with ID 42.
- `/api/users/42/orders` → Might represent orders placed by the user with ID 42.

A path tells you which resource you want to work with in an API. For example, `/api/users/42` points to a specific user.

Paths often contain both static and dynamic parts. In the example above:

* `/api/users/` is static - it's always written exactly this way
* `42` is dynamic - it changes based on which user you want

When you build an API, your framework (e.g. FastAPI in COMP423) handles **routing** - matching each incoming request to the right piece of code. The framework looks at two key inputs:

1. The path (what resource you want)
1. The HTTP method (what you want to do with it)

For instance, sending `GET` vs `DELETE` to `/api/users/42` will trigger different actions, even though the path is the same. GET might retrieve the user's information, while `DELETE` might remove their account.

## Query Parameters: Refining Requests

Query parameters let you customize what data you get back from an API. They are added to the end of a URL after the path and tell the server how to filter, sort, or modify the data before sending it back.

Here's how they work in a URL:

```
www.bookstore.com/books?genre=fantasy&sort=newest
```

The parts of a query parameter:

- The `?` shows where query parameters begin
- Each parameter has a name and value joined by `=` 
- Multiple parameters are connected with `&`

Common uses include:

- Filtering: `?genre=fantasy` (only return fantasy books)
- Sorting: `?sort=newest` (arrange by newest first) 
- Pagination: `?page=2&limit=10` (get 10 items from page 2)
- Searching: `?search=dragon` (find items containing "dragon")

The server reads these parameters and adjusts what data it sends back based on them.

Beyond APIs, query parameters are a fundamental part of the web. A common example is a Google search URL:

- `https://www.google.com/search?q=REST%20API` → Searches Google for 'REST API'. 

You, as the designer and implementer of an API, will decide whether any of your routes utilize query parameters and be responsible for modifying responses based on their values.

## Bodies: Sending Data in Requests

For **POST** (creating) and **PUT** (updating) requests, we need to send more complex data to the server. This data is sent in the **body** of the HTTP request, often in JSON format:

```json
{
  "name": "Alice",
  "email": "alice@example.com"
}
```

- `POST /api/users` → Creates a new user with data in the request body.
- `PUT /api/users/42` → Updates user 42 with new data.

Unlike paths or query parameters, **bodies are not visible in the URL** and can contain more complex structures. As such, sending requests to APIs with bodies takes writing some API integration code _or_ using a tool to form the request. We'll look at such a tool in the following reading in this sequence.

## Headers: Metadata for Requests

HTTP **headers** carry additional information about the request, such as:

- **Authentication** is used to prove the request is made by an authorized user (e.g. Bearer Tokens for security):

  ```http
  Authorization: Bearer abc123xyz
  ```

- **Content-Type** tells the server the data format in the request body (e.g., specifying JSON data format):

  ```http
  Content-Type: application/json
  ```

- **Accept** indicates the client expects a certain data type in the response body (e.g., requesting a response in a specific format):

  ```http
  Accept: application/json
  ```

Headers are used to address API concerns around security, content negotiation, and caching, among other things.

## Summary

When designing or using a RESTful API, it's crucial to understand how different inputs function:

- **Methods** define what action is being performed on the resource.
- **Paths** uniquely identify resources.
- **Query parameters** refine requests with filters and sorting.
- **Bodies** carry complex data for creating or updating resources.
- **Headers** provide metadata like authentication and content negotiation.

Each of these plays a role in designing APIs well. In the next part of this reading, you will actually _build_ a simple API that brings these concepts together.

