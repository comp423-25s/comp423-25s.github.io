# Sprint 1 - Week 1 - One Story End-to-End

The goal of this sprint is to have your primary story working end-to-end in the CSXL code base. Additionally, your team will get into the flow of performing Pull Requests and Code Reviews for one another and keeping your project board updated to reflect the status of ongoing work. Finally, you will stand up a staging environment that mirrors our production environment on CloudApps.

In the first week of this Sprint, your feature's backend services layer is permitted, and generally expected, to fake responses from the database and AI integration.

## Expectations

* One story end-to-end
* Project Management Standards
* PR/CR Workflow Enforced from Stage
* Running on CloudApps

## Your Primary Story

Focus on your team's most valuable user story that incorporates an AI integration. Your goal is to have this working and interactive from the front-end all the way to the back-end. In this first week, your focus is orienting yourself with existing code and finding where your team's work will fit into it. For now, your aim is to implement from the front-end down to the back-end services, but those back-end services will be faked. Next week, we can worry about the AI integration and data persistence concerns. Since this is a large code base, you can likely find examples of most everything you are trying to accomplish by looking around at other areas of the application and seeing how they were achieved there.

### Back-end API and Data Models

Your team should start with establishing your back-end Pydantic models and routes. The main entrypoint of the backend API is `backend/main.py`. You will see it imports its routes from FastAPI router files in the `backend/api` directory.

For now, we recommend establishing your team's own separate router file in an appropriate `backend/api` subdirectory (e.g. room/desk reservation logic is in `coworking`, courses is in `academics`, office hours queue is in `office_hours`). Start with a single test route to be sure you can get it working and showing up in the `localhost:1560/docs` OpenAPI interface. After defining a router and adding a route to it, be sure to register it in `backend/main.py` as a `feature_api`. Before continuing further, be sure you see your route appear in `/docs`.

Helpful hints:

* To run the development CSXL server, from the DevContainer, use `honcho start` and navigate to `localhost:1560`
* When in doubt, reset your database following the steps in `docs/database.md`
* Be sure to name your `APIRouter` instance in your backend router module `api`

Once you have a "hello world" route that you can successfully use from `/docs`, you are ready to start fully defining the routes you will need for this story. For now, we recommend focusing only on the routes you need for your initial story and no more. These routes will also need Pydantic models, or changes to existing models. You should make those changes in the `backend/models` directory. New models should be added to new files whose file structure is informed by where you chose to define your routes. If you need to "modify" an existing model, we recommend the approach you take for now is to use inheritance to define a new model just for your feature which extends the existing model and adds any additional fields needed. For an example of inheritance, see `backend/models/user_details.py` where `UserDetails` extends `User` and adds some additional fields to `User`.

The FastAPI routes you define and need for this story should follow the conventions we learned about annotating route parameters this semester. The conventions we have learned are newer (and better!) than the more dated style you are seeing in the CSXL code base. (We hope to update them this summer!) You should also include documentation for your route definitions like we expected during the FastAPI exercise earlier this semester.

### Back-end Services

Your routes should handle HTTP-level concerns, but ultimately should delegate the business logic to a service with necessary inputs coming from the request. You should define a new service similar to how other services are implemented in `backend/services`, also appropriately organized in the file structure, with the service methods your routes will depend on. For now, you can fake return values from these service methods in order to make progress on this project with your team. In doing so, the front-end work will be able to progress independently of the back-end and the API contract will be the shared agreement between layers of the stack.

### Front-end

How your team's front-end is organized will be highly dependent upon your feature and story. You should find your way to the components and widgets you are likely to integrate with. If you need an entirely new front-end route, take a look at how other features work in the codebase and find relevant examples to work off of. You should utilize Material UI widgets in your feature. If you're looking for how to utilize a specific widget, look for relevant examples.

While there may be a tendency to reach for GPT or Co-pilot on the front-end, please note it's likely to create a bigger headache and mess than you think if you lack confidence in the front-end. You are much better off trying to make slow, steady progress pair programming and having a sense of everything you are changing.

If you arrive in office hours with code for your feature which you cannot explain, the TAs are instructed to help you revert back to `stage` and ask you to go work on it more intently, to try again.

## Team Project Management

Continue utilizing the expected tools and workflow of the course:

1. Maintain your Project Board with cards linked to issues, assigned to team member(s), with descriptive titles for all cards/issues
2. Perform work on branches off of `stage`
3. Perform pull requests with well written titles and messages and request code reviews from team members
4. Make effortful and helpful code reviews for your team mates, helpfully maintaining high standards of code
5. Squash and merge approved PRs into `stage`

## PR/CR Settings

Let's setup your GitHub repository so that your team is able to work from a branch named `stage` as your primary branch. We will reserve the `main` brach to reflect production's `main` branch.

One member of your team should create a branch in the project named `stage` and push it to your team's repository. Other members of the team should `fetch` and switch to `stage`.

A member of the team should setup the branch protection rules for `main` and `stage` in your team repository. In your team's final project repository, navigate to:

1. Settings
2. General > Default Branch > Change Branch to `stage`
    * Press the **Swap Button (not the Pencil!)** and select `stage`
    * If you do not see `stage`, be sure you completed all the steps in "Initialize Team Repository", then refresh this page and try again.
    * Press Update and accept the change
3. Change to the Branches tab in the sidebar
    0. Add Branch Ruleset
    1. Ruleset Name: `main`
    2. Enforcement status: `Active`
    1. Targets, Add Target, Include by Pattern: `main`
    2. (Check) Restrict creations
    3. (Check) Restrict updates
    3. (Check) Block force pushes
    4. Save Changes with `Create` Button
4. Add another Ruleset (Go back to Rulesets tab)
    1. New Branch Ruleset
    1. Ruleset name: `stage`
    1. Enforcement status: `Active`
    1. Targets, Add Target, select **Include default branch**
    1. (Check) Restrict deletions
    4. (Check) Require linear history
    2. (Check) Require a pull request before merging
        1. (Check) Require approvals: 1 required
        2. (Check) Dismiss stale pull request approvals when new commits are pushed
        3. (Check) Require approval of the most recent reviewable push
        3. (Check) Require conversation resolution before merging
    6. Save Changes with `Create`

Your team repository now protects `main` from modifications and requires Pull Requests and Code Reviews on `stage`. This workflow is representative of many industrial workflow settings.

## Running on CloudApps

<!-- Before attempting to get your `stage` branch running on CloudApps, be sure you are locally working in your team's `stage` branch. -->
Update: Deploying to CloudApps will be reserved for next week, not the first week of this Sprint.