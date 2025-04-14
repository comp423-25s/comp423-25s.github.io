---
authors:
  - Kris Jordan
---

## Sprint 2 Expectations

This sprint is about arriving at a well implemented, tested, and thoughtful production-quality feature.

### Expectation 0: Complete 2nd End-user Story End-to-End

In addition to the first story from SP01, we expect a 2nd end-user story working end-to-end. This story may not depend upon the AI integration (and likely will not!). If your feature significantly involves two different personas, try choosing the most important story from your second persona.

### Expectation 1: Polish for End-user Stories

The interactions designed for your primary persona should be smooth and polished. This includes user-friendly interactions and form design, friendly error messages (or, even better, designing away the ability for there to even be errors!), and thoughtful user experience considerations such as clear user instructions and being sure everything visible works. If there are features you added user interface elements for that are not yet implemented, you should remove them by the end of this sprint. Everything visible should be functional.

Additionally, polish should be added to the implementation wherever possible. You are encouraged to move through each story of your feature from end-to-end, including docs and tests, and improve your implementation wherever possible.

### Expectation 2: Document Your Implementation for Future Developers

Your initial design document from SP00 set you out in a direction to head with respect to design and routes. It is highly likely that while your team moved in that direction, the hopes and dreams of the design document were met with the realities of time constraints and technical challenges that required some iteration and deviation from the original plan. This is both typical and _why overplanning without any implementation experimentation is rarely wise_.

For this expectation, draft a markdown document in the `docs/` directory of your project repository that is based on the realities of the feature work your team is doing and has implemented. It should be written for another developer to read and to understand how your feature is implemented. Structure it in such a way as to document how your feature works at each layer of the stack, from frontend to backend, database implications, and AI integration. Especially for the API, document the key routes and data models that exist in your code base. **These should be visible and readable, fully formatted, on GitHub after you push your branches.**

You should avoid language such as, "we would direct new developers to X and give them...". Your documentation is written directly addressing a developer, not the course staff. If it helps, imagine you are writing for a future COMP423 student working to understand and extend your feature. Include screen grabs of the primary components and/or widgets of the frontend and look for other docs files on how to organize and include screenshots in your markdown.

Choose plain language where possible.

Ensure the formatting of your document is easy to read and understand. Make appropriate use of paragraph text, versus merely bulleted lists, where needed. Choose heading text that is appropriate for your feature and the audience. If information would better be represented with a table, use a table instead of a list. This list is not exhaustive. Use your best judgement to make your documentation a _great_ artifact you can be proud of and share with future employers.

Finally, include screenshots of your feature from the end user persona's perspective with narrative of what is being shown. To add an image to your project, place it in the `docs/images` directory.

Include an authors section toward the top of the document listing the names of everyone in the group with links to their GitHub profiles.

This document should be written by your four group members as first authors. LLMs usage is only appropriate for copyediting and feedback on how your documentation could be improved.

### Expectation 3: Project Management & Standards

These remain the same as in the previous sprint.

Issues are kept up-to-date on project boards and closed out when completed. Changes are merged into stage exclusively via pull requests with meaningful code reviews. Commits merged into stage are descriptive following best practices of commit messages.

**Angular Material components are used anywhere there are inputs, tables, tabs, etc. If there is an Angular Material component that achieves what your user interfaces need, you should use it in the frontend rather than standard, or bespoke, native HTML controls.**

Backend service classes should be tested using Pytest with mock data. Backend service classes and methods should be documented using docstrings following the [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings). Your final backend service code files must maintain 100% passing test coverage on stage. Your feature can and should mock the OpenAIService with a fixture similar to how similar services are mocked (See `backend/test/services/fixtures.py` for an example of how `permission_svc` is mocked, for example. You will see just after how `user_svc` is mocked to depend upon this. Then see `backend/test/services/user_service.py`'s `test_list_enforces_permission` for an example of how the mock is used.)

Careful attention to permissions and access control should be paid to adhere to the principle of least privilege. Users should only be able to perform the necessary actions on the permitted resources for their legitimate purpose.

Stories merged in to `stage` should be of usable, production quality.

### Expectation 4. Running in Production on CloudApps

Instructions coming soon :)

### Extra-Credit (1 point) - Catch Your `stage` Branch up with csxl.unc.edu Production

The TAs are not permitted to assist with this extra credit opportunity in or outside of Office Hours.

Some PRs have landed in production at `csxl.unc.edu` since you began Sprint 2: <https://github.com/unc-csxl/csxl.unc.edu/commits/main>

To earn this point of extra credit, you should catch your stage branch up to `upstream/main` such that you have commit `cef9639`, or later, in your `stage` branch. You may need to resolve conflicts. When creating a PR for this catch-up branch, **rather than squashing and merging into your `stage` in this instance you should use the "Create a Merge Commit" strategy**. This strategy will retain the history from production's main branch.

This is good practice, and easier to achieve, if it's done semi-regularly. Additionally, this serves as a prerequisite to being considered for merging your team's features into production after the semester ends.