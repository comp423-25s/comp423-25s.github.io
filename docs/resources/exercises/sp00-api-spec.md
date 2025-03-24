# Sprint 1 - Week 1 - Interactive Prototypes & API Scaffolding

Now that your initial feature design document and mid-fi wireframes are in place, your team will extend your work by developing interactive, clickable prototypes. These prototypes will enable your team to clearly demonstrate user interactions, providing peers with tangible insights into your feature’s workflow. These clickable prototypes will be high fidelity and utilize the [Material 3](https://m3.material.io/get-started) design system of the CSXL. Additionally, you'll start scaffolding out the backend API routes and models necessary for your feature using FastAPI as the foundation.

## Assignment Details

### 1. Clickable Prototypes

- Choose your primary user stories and their wireframes from last week to convert them a **high fidelity wireframes** using the CSXL Figma template.
    - Duplicate the [CSXL Figma template](https://go.unc.edu/csxl-figma) into one of your team members' Figma spaces and add your team members to it.
    - Add your story to the `Getting Started + YOUR DESIGN` page. Copy starter pages from **CSXL Design** page and material components from **Components**.
    - We expect to see Material components and design principles followed throughout, unlike in the lo/mid-fi wireframes produced last week.
- Turn your high-fidelity wireframes into a clickable Figma prototype as shown in class.
- Clearly illustrate the interactions from your top 3 critical user stories.
- Ensure each clickable prototype demonstrates:
    - The sequence of interactions.
    - Where your feature will use the OpenAI LLM API.
    - Key interactions or critical decision points clearly presented.
- Link each clickable prototype clearly within your stories in your design document (Google Docs, Notion, etc.).
- Be prepared to present your top two story prototypes in a concise, 5-minute demonstration during class on Monday, March 31st. For each presented user story, include:
    - The associated REST API route(s) (next part).
    - The model(s) it utilizes.

!!! tip "Prototype Presentation Advice"

    - Clearly narrate your user's journey through your prototype.
    - Highlight the value your feature provides through specific interactions.
    - Keep explanations clear and to-the-point to manage your time effectively.

### 2. REST API & Model Scaffolding
Begin detailing your API in your design document by:

- Defining REST API routes that clearly support your critical user stories.
- Describing new models your feature's REST API will require. Refer to existing models located at `backend/models` in the CSXL repository to guide your designs.
- Clearly indicate if existing routes or models will need augmentation or modification.

Your document should clearly communicate:

- Route HTTP methods (`GET`, `POST`, `PUT`, `DELETE`) and paths.
- The purpose of each route.
- Basic descriptions of the data each route will handle.

### 3. Integration Analysis
Answer the following critical integration questions in your design document:

- **Existing Dependencies:**
    - Identify specific files, classes, and methods your feature's starting point will directly depend upon, extend, or integrate with. Provide permalinks (including line numbers) from the [CSXL codebase](https://github.com/unc-csxl).
- **API & Models:**
    - Clearly cite where you plan to add routes (new files are acceptable, but provide their complete paths).
    - Clearly cite where you plan to add or modify models.
- **Frontend Components:**
    - Identify how you will organize your frontend component(s).
- **AI Prompts:**
    - Behind the scenes your backend will need to make requests to the OpenAI API (think: ChatGPT). Start to brainstorm the prompts your backend routes will use with the AI, in other words: what would you put into the ChatGPT chat box and expect back. A common strategy is taking some user text and [converting it into a structured JSON output you specify](https://platform.openai.com/docs/guides/structured-outputs).
    - Identify at least one prompt and the JSON model format you expect as a response from the OpenAI API. Give some sample input text or JSON data from user inputs, representative of what your backend would send to the OpenAI API, and provide concrete examples of expected responses.

Clearly identifying these dependencies and frontend needs early will streamline your future development and avoid surprises.

### Project Management Best Practices

On Wednesday, we'll provide instructions for setting up your team's project board on GitHub. We'll dedicate class time on Friday to discussing GitHub issues and project board best practices. Keep the following in mind:

- Tasks should be clearly described, assigned to team members, and updated frequently.
- Link each project board card to relevant issues in your GitHub repository.

!!! success "Why Project Management Matters"

    Using structured project management practices from the beginning will improve:

    - Team coordination and productivity.
    - Code quality through continuous peer review.
    - Your shared understanding of the codebase and collaborative skills.

### Submission & Demonstration

- Continue updating your original design document.
- Ensure all clickable prototypes are clearly linked to from within your user stories. Test these links in an incognito window. You should be taken directly to the start of a story clickthrough.
- Be ready to demonstrate your clickable prototypes clearly and succinctly on March 31st, including clear references to routes and models associated with each story.

This structured, design-forward approach will enhance both the quality and manageability of your feature, setting a strong foundation for the development cycles ahead.