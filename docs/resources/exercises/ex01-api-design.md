# EX01. API Design and Implementation

## App Overview: The Pastebin + URL Shortener

In this lab, you and a partner will collaborate to design and implement a service that combines:

* A Pastebin-style API (store text snippets and retrieve them by a unique URL).
* A URL-shortening API (submit a long URL and receive a short, redirectable URL).

These two functionalities must share a common **opaque namespace** for links, presenting unique design challenges regarding how to store and retrieve resources come implementation time.

This application will be able to generate shortened URLs for sharing content such as:

1. Pastbin Example: <https://pastebin.com/uYCCWaxy>
2. URL Shortener Example: <https://go.unc.edu/Xj9b6>

This application design will feature three personas:

1. **Sue Sharer** is someone who wants to distribute content easily. She might be a student sharing notes, a blogger sharing a quote, or a prankster hiding a Rickroll. Sue values convenience, control, and customization, which is why she wants options like vanity URLs and expiration times.

2. **Cai Clicker** is the person who receives and opens links. They might be a friend reading a shared snippet, a recruiter reviewing a resume, or an unsuspecting victim of a disguised meme. Cai values seamlessness and reliability—when they click a link, they expect to either see content immediately or be redirected without confusion.

3. **Amy Admin** is responsible for monitoring and managing all active resources. She is a community manager. Amy values visibility, control, and order, ensuring that shared content remains appropriate and awareness of high-traffic links.

### User Journey Examples

An journey may combine a few user stories in order to give a complete start-to-finish example of a feature's use. Since you may not be familiar with the point of a pastebin-like service, or URL shortener, consider these journeys before reading the user stories in a more standalone presentation.

### **Sue Sharer Creates a Text Snippet**  

1. Sue wants to share a quote from a book with a friend.  
2.  She submits the text:  
    ```
    "Not all those who wander are lost."
    ```
3.  The system generates a random URL path and responds with the information Sue needs to share the URL.
4.  Sue shares this link with her friend.  
5.  Cai Clicker clicks the link, which looks something like `https://<your-apps-hostname>/xYzA12` and sees the text snippet:  
    ```
    "Not all those who wander are lost."
    ```

### **Sue Sharer Creates a Shortened URL**  

1. Sue wants to prank a friend by disguising a Rick Astley video link.  
2. She submits the long URL below and a vanity path of `exam-solutions`:  
    ```
    https://www.youtube.com/watch?v=dQw4w9WgXcQ
    ```
3.  The system generates a link with the vanity path and responds with the information Sue needs to share the URL. 
4.  Sue shares this link with her friend: `https://<your-apps-hostname>/exam-solutions`  
5.  Cai Clicker clicks the link and is redirected to:  
    ```
    https://www.youtube.com/watch?v=dQw4w9WgXcQ
    ```

### Required User Stories

1. Sue Sharer
    1. As Sue Sharer, I want to create a new text snippet with an optional expiration time and the ability to request a custom vanity URL, so that I can control how long it is available and share a more meaningful link.
    3. As Sue Sharer, I want to create a shortened URL with an optional expiration time and the ability to request a custom vanity URL, so that I can control how long it is available and share a more meaningful link.
2. Cai Clicker
    1. As Cai Clicker, I want to open a shared text snippet by clicking its unique link, so that I can read the content that was provided to me.
    2. As Cai Clicker, I want to open a shortened URL by clicking its unique link, so that I am automatically redirected to the original long URL.
2. Amy Admin
    1. As Amy Admin, I want to see a list of all active resources (text snippets and shortened URLs) and filter by type or view counts greater than some low threshold, so that I can oversee what content is currently being shared.  
    2. As Amy Admin, I want to see how many times each resource has been accessed, so that I can monitor usage and identify high-traffic resources.
    3. As Amy Admin, I want to update the content of an active text snippet or change the target of a shortened URL, so that I can correct or modify existing resources when necessary.  
    4. As Amy Admin, I want to delete any active resource from the system, so that I can remove content that should no longer be available.  

### Path Requirement Specifications

User stories 2.a. and 2.b. above are the only stories which we will very specificaly share an API requirement, as follows:

These two stories should both share the same opaque route, including *method* and *path*. The method is `GET` and the `path` in FastAPI route path syntax is `/{resource_identifier}`. This means that there is a single shared path pattern for retrieving both types of resources.

* When a user accesses a generated or vanity URL, the system must determine whether it corresponds to a **text snippet** or a **shortened URL**.
* The user **should not** be able to infer whether a given URL points to a text snippet or a redirection just by looking at it.

### No Authentication Enforced

The concerns of how to authenticate a user, like Amy Admin, and authorize various actions, is beyond your concern in this initial API design. You should proceed with all routes publicly available, unprotected. Later, we'll learn strategies for authenticating and authorizing various actions at the HTTP API level.

## Phase 1: API Design

### Getting Started

To begin work on EX01, you and your partner will need to accept a GitHub classroom with your ==assigned Team Name (in the form of **`team_0_NN`**)== found in the [pairings sheet](https://docs.google.com/spreadsheets/d/1Eu4tzOATIkRDSwg9NqxZleKChDjYhuQD8ORjOspsNMg/edit?usp=sharing). First look up your team name by your PID and copy it. Then go accept the [GitHub classroom assignment by following this link](https://classroom.github.com/a/FfbRXDWO). Search for your team name and if it already exists, join it. If you are the first of your pair to begin, create the team with the assigned team name.

### Create a Branch for Individual API Design

Clone the project, open your project in a dev container, and create a branch for your individual API design. **Name your branch something that includes your onyen or github username.**

### Individual API Design

In your branch created above, go ahead and stub out an HTTP API, making use of FastAPI routes and Pydantic models as necessary, that satisfy the user stories. Use the `/docs` user interface to review your routes. Your objectives are:

* **Define Endpoints**: Specify all the HTTP routes required for the required user stories above.
* **Design Data Models**: Create Pydantic models that define the structure of request bodies and responses.
* **Clear OpenAPI Documentation**: Fully document important your API using the OpenAPI standards discussed below.
* **Establish Conventions**: Ensure consistent naming and documentation throughout your API design.

### OpenAPI Specification Requirements

FastAPI and Pydantic have special constructs which allow you to more fully specify your API and its documentation to produce the standards-based `OpenAPI.json` spec powering the `/docs` user interface.

You are required to add specification and documentation to your API design along each of the following dimensions. You can find examples of how each is done following this overview list:

- **FastAPI Application:** Ensure your app is properly instantiated with required metadata.
- **Route-level:** Always include a summary and description; document response bodies thoroughly.
- **Route Parameters:**
    - **Path parameters:** Must have descriptions and optional validations.
    - **Query parameters:** Must have descriptions and can include validations.
    - **Body parameters:** Must have descriptions and `openapi_examples` for clear request body documentation.
- **Pydantic Fields:** Every field should include a description and an example (or examples) to aid API consumers.

#### FastAPI Application-level Documentation

Instantiate your FastAPI app using the `FastAPI` constructor. You *must* provide a `title`, `contact`, `description`, and `openapi_tags` (for organizing routes), as shown below. Notice that the description is markdown and you can use a _docstring_ to give your API documentation 

*Example:*

```python
app = FastAPI(
    title="EX01 API Design",
    contact={
        "name": "Parter A, Partner B",
        "url": "https://github.com/comp423-25s/<your-team-repo>",
    },
    description="""
## Introduction

Your introduction text to your API goes here, in **markdown**.
Write your own brief intro to what his API is about.
""",
    openapi_tags=[
        {"name": "Sue", "description": "Sue Sharer's API Endpoints"},
        {"name": "Cai", "description": "Cai Clicker's API Endpoints"},
        {"name": "Amy", "description": "Amy Admin's API Endpoints"},
    ],
)
```

After you've more fully configured your `app`, as shown above, try reloading your OpenAPI UI by navigating to `/docs` in your dev server. You should see the information above being used to improve the documentation generated. The **tags** added will allow you to organize your routes based on the intended user. In real APIs, tags are generally used to cluster endpoints for a specific feature together; here we're using them to organize by persona served.

#### Route-level Decorator Specification

Define endpoints using FastAPI’s route decorators (e.g., `@app.get`, `@app.post`). Each route *must* include a *summary*, *description*, and *tag*. The *tag* corresponds to the `openapi_tags` you specified above and will be a persona name. If your route returns response codes besides `200`, such as `404`, you need to specify the *responses* field as shown below. For a given status code, the *description* is required and the *model* (Pydantic subclass) is only necessary if the response returns a body.

*Example:*

```python
from typing import Annotated

class MessageResponse(BaseModel):
    message: Annotated[str, Field(
        description="Information conveyed ot user", examples=["Hi!"]
    )]

# ...

@app.get(
    "/items/{item_id}",
    summary="Retrieve an Item",
    description="Get details of an item by its ID.",
    responses={
        404: {
            "description": "Item not found",
        }
    },
    tags=["Shopping"]
)
def get_item(item_id: int) -> MessageResponse:
    if item_id > 0:
        return MessageResponse(message="Item found!")
    else:
        raise HTTPException(status_code=404, detail="Item not found!")
```

#### Dynamic **Path** Parameters

For dynamic segments in the URL (path parameters), use `Path`. Include a **description** and any additional keyword parameters found in the [official documentation](https://fastapi.tiangolo.com/reference/parameters/?h=path%28#fastapi.Query) you believe would be helpful in specifying and documenting your path (useful ideas: 1. `examples` list of example values you might expect for the parameter, 2. validation such as `min_length` or `gt` (greater than) as shown below).

*Example:*

```python
from fastapi import FastAPI, Path
from typing import Annotated

# ...

@app.get("/users/{user_id}")
def get_user(
    user_id: Annotated[int, Path(
        description="The unique ID of the user",
        gt=0,
        examples=[1, 423]
    )]
) -> User:
    ...
```

#### **Query** Parameters

For query parameters (appended to the URL), use `Query`. Each query parameter **must** include a **description**, **should** probably include a default value, and can optionally include additional examples and validation rules, if needed. See the [official documentation](https://fastapi.tiangolo.com/reference/parameters/?h=path%28#fastapi.Query) on supported keyword parameters when specifying and documenting query parameters.

*Example:*

```python
from fastapi import FastAPI, Query
from typing import Annotated

# ...

@app.get("/search")
def search_items(
    q: Annotated[str, Query(
        description="The product search query",
        examples=["jordans"]
    )] = "" # Default value is empty string
) -> SearchResults:
    ...
```

#### Documenting Pydantic Model Fields

Within your Pydantic models, use the `Field` function to document each field. Every field **must** have a **description** and **examples** list to aid API consumers.

*Example:*

```python
from pydantic import BaseModel, Field
from typing import Annotated

class Item(BaseModel):
    name: Annotated[str, Field(
        description="Name of Product",
        examples=["UNC Jersey", "UNC Socks"]
    )]
    price: Annotated[float, Field(
        description="Sales Price",
        examples=[75.0, 20.0]
    )]
```

#### Request Body Parameters

For request body parameters (used in `POST`/`PUT`/`PATCH` requests), define a Pydantic model and use `Body` to add metadata. The body parameter **must** include a **description** and **openapi\_examples**. These examples help make testing out the API in `/docs` easier, as you will see when you try it out. You can also add validation rules if needed.

*Example:*

```python
from fastapi import FastAPI, Body
from typing import Annotated

# ... Same Item model as above ...

@app.post("/items")
def create_item(
    item: Annotated[Item, Body(
        description="The product to create",
        openapi_examples={
            "Air Jordans": {
                "summary": "Air Jordan 1 Mid SE",
                "description": "Sample product to create",
                "value": {
                    "name": "Air Jordan 1 Mid SE",
                    "price": 134.99
                },
            }
        }
    )]
) -> Item:
    ...
```

### A Note on Model Design and Typing

In your design space for implementing your models, there are likely a few paths worth considering. The design challenge you are confronted with is your API involves two different kinds of resources (text versus links) and they have differences in behaviors, validations, and so on. However, they also share some things in common (such as the namespace for their shortened paths once created, the expiration, and the access counter).

1. (Do not do this!) Unimodel - Single model shared by both types. This solution is taking on technical debt and becomes gnarly to maintain and extend.
2. (Discouraged) Traditional Inheritance Hierarchy - a single resource subclass of `BaseModel` which is then subclassed for your specific resources.
3. (Recommended) [Discriminated Type Unions](https://docs.pydantic.dev/2.0/usage/types/unions/#discriminated-unions-aka-tagged-unions) - models use a common string field to communicate their type. This is useful because once a Python or JavaScript object is _serialized_ into JSON for transfer over an API, only the field names and values of an object are transferred. By encoding type into a field, it's easier to work with. This strategy has emerged in many dynamic programming languages and is recommended in Pydantic and our front-end language TypeScript.

To learn more about discriminated type unions in Pydantic, see the [official Pydantic documentation]. Additionally, feel free to search or have an [interactive learning conversation with ChatGPT](https://chatgpt.com/share/679f8a1a-22d4-8000-8ad4-2dab48c5f46f) to gain a better understanding. [Here's an example prompt I tried in ChatGPT that gave a solid introduction](https://chatgpt.com/share/679f8a1a-22d4-8000-8ad4-2dab48c5f46f). As always, with LLM generated content, read carefully and vigilantly: it doesn't always tell the complete story, the correct story, or have a complete understanding of what exactly you are doing.

### Collaboration for Phase 1

Each of you should individually draft a design of your API in FastAPI on your own branches (branch naming specified after the Getting Started section above). You should both push your branches to GitHub.

Once you are ready to merge your branches to form a unified API for your team, we do not recommend actually attempting a merge in `git`. You are welcomed to, but at your own peril. Since you both worked in `main.py`, and made design decisions independently, the merge conflict resolution will be gnarly.

Instead of attempting a `git` merge, we strongly suggest **pair programming**, and starting over by going back to your `main` branch on one of your machines. Start a new branch based on `main` that is `pair-api-design`. On the other of your machines, have open both of your branches in GitHub to easily view how each of you approached the design and try to form a consensus on how to approach. You will be well served by each reading each other's design and then attempting to whiteboard your final approach before diving into code. Once you are complete, push your final `pair-api-design` to GitHub and submit your teams' reflection for Phase 1 on Gradescope.

### Sanity Checks

Questions to consider in the context of your API:

* Have we ensured that our design addresses every required user story for Sue, Cai, and Amy?
* Are our naming conventions for endpoints, models, and fields consistent and descriptive enough for all personas?
* How does our design distinguish between a text snippet and a URL shortener resource when using the same `/{resource_identifier}` endpoint?
* Are we including required metadata (e.g., summaries, descriptions, examples) for every endpoint and model field so that a developer can easily understand our API?
* How have we documented error responses (like 404 for missing resources) in our endpoints?
* Is the route design intuitive for both API users and maintainers?
* How easily can our design be extended in the future if new requirements are added?

### Phase 1 Submission and Reflection Questions

* Gradescope submission will include:
    * Permalink to branches of both partners
    * Permalink to the final `pair-api-design` branch
* Brief reflection question:
    * What challenges did we encounter when comparing our individual designs, developing a single design, and pair programming our joint, final design?

### Key Takeaways

1. **Designing for Users and Use Cases**

    Through this assignment, you’ve gained experience designing an API that serves multiple types of users with distinct needs. You’ve seen how clear, user-focused API design is essential—not just for making the system functional but also for ensuring a smooth experience for different personas. This mirrors real-world software development, where balancing the needs of end users, system administrators, and stakeholders is key to building successful products.

2. **Writing Clear, Professional API Documentation**

    By leveraging FastAPI’s OpenAPI documentation, you’ve practiced writing API specs that go beyond just making things work—you’ve created an API that is easy for others to understand, test, and use. In industry, well-documented APIs are what enable teams to scale, integrate with other systems, and onboard new developers quickly. This attention to detail will serve you well in any software engineering or product development role.

3. **Navigating Design Constraints**

    You’ve tackled a unique design challenge: storing and retrieving two different resource types (text snippets and URL redirects) while keeping a shared, opaque namespace. This required careful thinking about data modeling and routing logic. Real-world software design often involves trade-offs like these, where multiple features must coexist within a unified system without exposing unnecessary complexity to users.

4. **Collaborating on Software Design in a Team Environment**

    By independently designing an API and then merging your ideas into a unified implementation, you’ve practiced an essential part of professional software development: balancing individual contributions with collaborative decision-making. You’ve navigated trade-offs, discussed design choices, and worked toward a shared vision—skills that are essential in any software engineering role.

5. **Designing the Interface First for Human-Centered Development**

    By focusing on the API interface before implementation, you’ve embraced a human-centered approach—prioritizing how users interact with the system rather than getting lost in internal details. This ensures the design is intuitive, valuable, and easy to integrate. A well-defined interface also enables parallel development: frontend teams can build against the spec while backend teams implement functionality, making collaboration more efficient. In real-world projects, this approach reduces wasted effort, improves usability, and accelerates development, ultimately leading to better software.