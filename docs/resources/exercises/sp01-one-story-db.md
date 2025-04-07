# Sprint 1 - Week 1 - One Story End-to-End with DB

The goal of this sprint, which began last week, is to have the most valuable story of your project demonstratable end-to-end from front-end down to the database and OpenAI AI integration. On Monday, April 14th, your team will demo the story running on CloudApps.

Requirements:

* Integration with OpenAI API
* Persistence in DB
* Project Management Standards
* Running on CloudApps

## Integration with the OpenAI API

On Friday, April 4th, API keys for all teams were handed out. Each member has an individual API key for use in their own development environment which is tracked for appropriate, class-related purposes. Additionally, we showed the strategy and some helper functions for integrating with the API in your projects. See the recording for a more fullsome description, but in general here are some useful links:

* [Backend OpenAI Service Helper](https://github.com/unc-csxl/csxl.unc.edu/blob/openaiservice/backend/services/openai.py)
* [Backend Service Using OpenAI Service](https://github.com/unc-csxl/csxl.unc.edu/blob/openaiservice/backend/services/health.py#L42-L47)
* [Backend Model for OpenAI Response Example](https://github.com/unc-csxl/csxl.unc.edu/blob/openaiservice/backend/models/openai_test_response.py)
* [Backend API Route using the Service](https://github.com/unc-csxl/csxl.unc.edu/blob/openaiservice/backend/api/health.py#L38-L44)

The path toward adding this sample code to your project shown in class involved adding the primary CSXL repository as a remote named `upstream` and using the `cherry-pick` subcommand to pull in the commit which added the above files. If you'd prefer to add these files manually to a branch, that's OK, too. The strategy shown in class was:

~~~bash
git remote add upstream https://github.com/unc-csxl/csxl.unc.edu.git
git remote fetch upstream
git switch stage
git switch -c add-openai-code
git cherry-pick 3c0822e
git push origin add-openai-code
~~~

Additionally, in your `backend/.env` file, you need to add an environment variable named `UNC_OPENAI_API_KEY` which is set to your personal key (no spaces) as handed out in class. **This is a secret and should not be committed to your team's repository!**

Then, create a PR which targets your `stage` branch for integration and have a team member review and merge.

**Notably: You need to rebuild your devcontainer via the command palette in VSCode so that you get the updated `backend/requirements.txt` dependencies which includes [`openai`](https://github.com/unc-csxl/csxl.unc.edu/blob/openaiservice/backend/requirements.txt#L15)**

**Requirement:** If your Sprint 1 end-to-end story integrates with the AI; great! You should be able to show your own backend service that injects the `OpenAIService` and makes use of its `prompt` method with a Pydantic `response_model` parameter type of your team's design. If your Sprint 1 story _does not_ have the AI integration in the user experience path, you should go ahead and write an additional backend API route and respective service that *will* integrate with the AI API and can be demonstrated via the `/docs` interface.

## Persistence in DB

Your story should involve persisting new information to the database in some way; specifically, and technically, this means either augmenting an existing `Entity` or introducing one or more of your own `sqlalchemy` `Entity` classes.

If your story (or feature!) does not necessarily directly involve new data in the database, then for this requirement, your team should introduce some sort of "AI Audit Log" entity that persists the user prompt and JSON-encoded text response from the AI. You do not need to surface this to an API/UI for this Sprint, but the successful recording of data should be visible via the Postgres Explorer plugin shown in class on Monday, April, 7th.

Database persistence integration should only occur in the backend services layer, not directly from the HTTP API layer.

If your demo / feature depends on data being prepopulated in your database, then you should add it such that running `python3 -m backend.script.reset_demo` repopulates your database correctly for demo purposes. When TAs are grading, they will run this script on your

## Project Management Standards

Continue utilizing the expected tools and workflow of the course:

1. Maintain your Project Board with cards linked to issues, assigned to team member(s), with descriptive titles for all cards/issues
2. Perform work on branches off of `stage`
3. Perform pull requests with well written titles and messages and request code reviews from team members
4. Make effortful and helpful code reviews for your team mates, helpfully maintaining high standards of code
5. Squash and merge approved PRs into `stage`

## Running on CloudApps

* Instructions for CloudApps deployment will post on Wednesday, April 9th.
