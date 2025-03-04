# 0. Layered Architecture: Separating Concerns in Software Design

## Introduction

One of the oldest and most enduring patterns in software engineering is a **layered architecture**. This pattern enforces a structured separation of concerns, ensuring that each layer of a system only depends on the layer directly beneath it. This approach provides a clean interface between layers and serves as a **barrier of abstraction**, helping to manage complexity in large systems.

A crucial characteristic of layered architecture is that **lower layers should have no explicit awareness of higher layers**. This means that a foundational layer does not (and should not) know of the higher-level layer that is built on top of it.

In this chapter, we will explore layered architecture in the context of building a **RESTful backend API using FastAPI**. Our goal is to introduce a new layer: **the business logic services layer**, and understand why it exists and how it interacts with the other layers. Our HTTP API layer will sit above your services layer.

## What is Business Logic and Why is it Called That?

The services layer is commonly referred to as **business logic** because it encapsulates the domain-specific rules and workflows that define how an application operates. The term "business logic" stems from the idea that this layer contains the core operations that are specific to the application's purpose or domain, regardless of the technical details of how the system is accessed or stored.

More precisely, business logic models the application's concerns using **simpler, domain-specific objects** rather than exposing technical implementation details. By defining these rules in a dedicated layer, we create a more maintainable and adaptable system. This separation allows us to reason about and modify the business rules independently from concerns like HTTP, databases, or external APIs.

The **HTTP routing layer** serves as a **facade** or **translator** between the external world and the business logic. This means that it handles incoming requests, extracts necessary parameters, calls the relevant business logic methods, and then formats the output for delivery as an HTTP response. This layer should contain minimal logic of its own, acting only as an interface to the core functionality contained within the business logic layer.

## Motivation for Separating Concerns

So far, we have learned about FastAPI routes and how they handle HTTP requests. However, if we write all of our application logic directly inside route handlers, several problems arise:

1. **Tightly Coupled Code** – Routes become entangled with business logic, making the system difficult to modify or extend.
2. **Lack of Reusability** – Business logic cannot be easily reused in different contexts, such as CLI tools or background tasks.
3. **Difficult Testing** – Testing business rules independently becomes challenging when they are mixed with HTTP concerns.
4. **Error Handling Complexity** – If errors are raised directly within route handlers, managing appropriate HTTP responses becomes messy and inconsistent.

To address these issues, we introduce a **business logic services layer**, which abstracts core application logic away from the HTTP concerns handled by FastAPI routes.

## Layered Architecture in Our API

Our API will follow a **three-layered architecture**:

1. **Routes Layer (Facade/Translator Layer)**: Responsible for handling HTTP requests and responses. Routes gather inputs, call services, and return results.
2. **Service Layer (Business Logic Layer)**: Contains core domain logic, independent of FastAPI. This layer operates on domain-specific objects and ensures that all application logic is encapsulated here.
3. **Persistence Layer (Coming Soon)**: Handles persistence and retrieval of data, usually via a database. Services interact with this layer but do not concern themselves with storage details. At this stage in the course, we will not concern ourselves with persistence. However, know that it is on the horizon and will be another layer!

### Simplified Example: User Lookup

Consider the following, hand-waving example of what this architectural strategy is trying to avoid:

#### `main.py` Without a Services Layer (Bad Practice)

```python
from fastapi import FastAPI, HTTPException
from models import User

app = FastAPI()

@app.get("/users/{user_id}")
def get_user(user_id: int):
    user = database.find_user_by_id(user_id)
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    return user
```

In this example, the route directly accesses the database, performs a lookup, and raises an HTTPException. This mixes **business logic (checking if the user exists)** with **FastAPI-specific concerns (HTTPException and status codes)**.

#### `main.py` with a Services Layer (Better Practice)
```python
from fastapi import FastAPI, HTTPException
from services import UserService, UserDoesNotExistError
from models import User

app = FastAPI()
user_service = UserService()

@app.get("/users/{user_id}")
def get_user(user_id: int):
    try:
        return user_service.get_user(user_id)
    except UserDoesNotExistError:
        raise HTTPException(status_code=404, detail="User not found")
```

#### `services.py` the Service Layer (Better Practice)

```python
from models import User

class UserDoesNotExistError(Exception):
    pass

class UserService:
    def get_user(self, user_id: int) -> User:
        user = database.find_user_by_id(user_id)
        if not user:
            raise UserDoesNotExistError("User does not exist")  # Raises a domain-specific error
        return user
```

### Key Improvements:

1. The service layer has no knowledge of HTTP. It operates purely on domain-specific objects and raises domain-specific errors (e.g., `UserDoesNotExistError`).
2. The route handler translates errors into HTTP responses, keeping the API’s interface clean and predictable.
3. Business logic is reusable. The UserService could be used elsewhere, such as in a background job, without modification.

### Sequence Diagram of Layers

Study the following diagram to understand how these layers serve specific purposes and separate concerns.

~~~mermaid
sequenceDiagram
    participant Client
    participant API Routing
    participant Service

    Client->>API Routing: HTTP GET /users/1
    API Routing->>Service: UserService.get_user(1) 
    Service-->>API Routing: User Data Model or throws UserDoesNotExistError
    API Routing-->>Client: 200 OK (JSON user data) or 404 Not Found
~~~

## Characteristics of a Well-Separated Service Layer

When designing a services layer, keep the following principles in mind:

- **No FastAPI-specific imports**: The service layer should **not** reference HTTP concerns (e.g., `HTTPException`).
- **Works with domain models**: Services should receive and return domain-specific objects (e.g., Pydantic models) instead of raw dictionaries or ORM objects.
- **Handles business rules**: Any domain-specific logic (e.g., checking if a user has permission to perform an action) should reside in the service layer.
- **Raises domain-specific errors, not HTTP exceptions**: The service layer should raise application-specific exceptions, like `UserDoesNotExistError` or `ValueError`. The routes layer should handle them and translate them into proper HTTP responses.

## Looking Ahead: Dependency Injection

So far, our `UserService` is instantiated directly inside our routes file. This works but is not scalable. In the next reading, you will be introduced to **Dependency Injection**, a technique that allows us to cleanly manage dependencies like `UserService` while keeping our code modular and testable.