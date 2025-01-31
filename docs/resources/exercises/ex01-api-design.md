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
    1. As Amy Admin, I want to see a list of all active resources (text snippets and shortened URLs), so that I can oversee what content is currently being shared.  
    2. As Amy Admin, I want to see how many times each resource has been accessed, so that I can monitor usage and identify high-traffic resources.  
    3. As Amy Admin, I want to update the content of an active text snippet or change the target of a shortened URL, so that I can correct or modify existing resources when necessary.  
    4. As Amy Admin, I want to delete any active resource from the system, so that I can remove content that should no longer be available.  

### Path Requirement Specifications

User stories 2.a. and 2.b. above are the only stories which we will very specificaly share an API requirement, as follows:

These two stories should both share the same opaque route, including *method* and *path*. The method is `GET` and the `path` in FastAPI route path syntax is `/{resource_identifier}`. This means that there is a single shared path pattern for retrieving both types of resources.

* When a user accesses a generated or vanity URL, the system must determine whether it corresponds to a **text snippet** or a **shortened URL**.
* The user **should not** be able to infer whether a given URL points to a text snippet or a redirection just by looking at it.


## Phase 1: API Design

The specifications for Phase 1 will be shared as soon as they are finalized and a Canvas post will be made. At a high-level, here is what you can expect:

- Specifications will be written as FastAPI route stubs (and therefore Pydantic models)
    - A route stub contains all method, path, query parameter, and request body schema necessary
    - A route stub contains response code and response body schema necessary
    - A route stub can use `...` or throw a not implemented exception; no implementation is expected.
- Each pair member will create their own first draft of the API specification in separate branches, independently.
- Pair members will then come together and create a unified API specification together, pair programming.
- Some reflection will be expected on the design process with some attention paid to evolution between individual thinking and the combined design.

More detailed specifications will be shared as soon as they finalize. In the interim, you can begin working through this exercise on paper like we practiced in class today. In fact, this is often a great way to start!

## Phase 2: API Implementation

Following submission of your individual and shared API specification drafts, you will work together to implement your API specification. Additionally, you will be writing route tests to verify your implementation. Finally, you will deploy your implementation to a cloud platform we will discuss in class next week.