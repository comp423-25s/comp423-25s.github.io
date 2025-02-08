# 1. Introduction to Dependency Injection in FastAPI

## What is Dependency Injection?

**Dependency Injection (DI)** is a widely used design pattern that promotes modular, testable, and maintainable code. It is a **core principle** in many modern application frameworks across various programming languages, including Java (Spring), Python (FastAPI), and TypeScript (Angular). The primary idea behind DI is simple: instead of constructing dependencies _inside_ a function or class body, we declare them as _special parameters_ and the DI system automatically constructs/manages the argument values and "injects" them as arguments. This process is called **dependency injection**.

This concept plays a role in modern **layered architectures** like you just read about. We previously introduced a **business logic services layer** that encapsulates domain-specific logic and separates it from the **routing layer** (which handles HTTP requests and responses). Dependency Injection provides a **clean and structured way** to introduce and manage dependencies between these layers, keeping them **loosely coupled** and testable.

### Why Use Dependency Injection?

Dependency Injection helps solve common software design challenges, making applications:

- **More Maintainable**: By loosely coupling dependencies between parts of a system, changes in one part of the application don’t require modifying other parts. Loosely coupled components make it easier to replace or upgrade individual parts of a system without affecting the rest. This reduces the risk of unintended side effects and promotes a more modular and extensible architecture.
- **Easier to Test**: With DI, dependencies can be replaced with mock implementations, making unit tests isolated and reliable.
- **More Flexible**: By programming to an interface rather than a concrete implementation, different implementations of a dependency can be injected dynamically, allowing for easy configuration changes.
- **Reduces Code Duplication**: Centralizing dependency management prevents repeated instantiation of services throughout the codebase.

You’ve actually already encountered DI in FastAPI! Every time a request includes **path parameters, query parameters, or request bodies**... where did the argument values come from? FastAPI injected them into your route handlers automatically! Now, let’s take this one step further: what if we wanted to inject a **custom service** into our application to handle business logic?

## How Does Dependency Injection Work in FastAPI Routing?

When a request is received in a FastAPI application, the following steps occur:

1. **Request Routing**: FastAPI matches the incoming request's URL and HTTP method to the appropriate route handler function.
2. **Dependency Resolution**: Before calling the route function, FastAPI checks for any declared dependencies using `Depends()`. It determines what dependencies are needed and resolves how to instantiate them in a correct order. An injected dependency may have its own injected dependencies that need to be resolved and instantiated first!
3. **Dependency Instantiation**: If a dependency is a class or function, FastAPI instantiates it (if needed) and injects it into the route function.
4. **Function Execution**: The route function is called with the injected dependencies passed in as arguments to the routed function's parameters.

## Example Implementation of Dependency Injection in FastAPI

Now, let’s apply these principles by using **dependency injection** in a FastAPI application to inject a service layer component.

### Step 1: Creating a Counter Service Protocol and Implementation

In Python, a **protocol** is similar to an interface in Java. It defines a contract that a class must follow without enforcing inheritance. This allows for better flexibility and testability.

Imagine a module named `services.py` that implements a counter service which keeps track of how many times its `increment` method is called and returns the count when called:

```python
# services.py
from typing import Protocol

class CounterServiceProtocol(Protocol):
    def increment(self) -> int:
        ...  # This method must be implemented

class InMemoryCounterService(CounterServiceProtocol):
    def __init__(self):
        self.count = 0  # Maintaining state in memory (not for production!)

    def increment(self) -> int:
        self.count += 1
        return self.count

# Creating a global instance of a counter service so that it resides in 
# the service module's "global" memory.
counter_service: CounterServiceProtocol = InMemoryCounterService()
```

Here you see `CounterServiceProtocol` defined _as a `Protocol`_ and `InMemoryCounterService` declared to explicitly implement this protocol. Note: unlike Java interfaces, you do not need to make this declaration explicit in Python, but here we choose to do so 

### Step 2: Injecting the Service with `Depends()`

Now, let’s use this service in our FastAPI app. Consider the following `main.py`:

```python
# main.py
from fastapi import FastAPI, Depends
from services import counter_service, CounterServiceProtocol

app = FastAPI()

# Dependency Injection Provider
def counter_service_provider() -> CounterServiceProtocol:
    return counter_service  # Injects our global CounterService instance

@app.get("/count")
def read_count(counter: CounterServiceProtocol = Depends(counter_service_provider)):
    """Increment and return the current count."""
    new_count = counter.increment()
    return {"count": new_count}
```

### Step 3: Understanding the Dependency Injection Flow

1. When a request is made to `/count`, FastAPI sees that `read_count()` depends on `counter_service_provider`.
2. It calls `counter_service_provider()`, which returns our global `counter_service` instance.
3. The injected `counter_service` is then used to update and return the counter.

Can you spot the **really big** 

---

### Step 4: Why This is Useful

- **We can easily swap out `InMemoryCounterService`** with a different implementation later (e.g., using a database instead of in-memory storage).
- **It makes testing easier**—we can inject a mock version of `CounterService` for unit tests.
- **Encapsulates state in a service rather than the route**, keeping our API layer clean.

This naturally follows from our previous discussion on **layered architecture**. Dependency Injection is the mechanism that allows us to decouple these layers cleanly and enforce the separation of concerns.

