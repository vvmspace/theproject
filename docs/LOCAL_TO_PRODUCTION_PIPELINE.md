# Local to Production: how features move from local to production environments

Read [ENVIRONMENTS.md](ENVIRONMENTS.md) for environments list and description.

## Task statuses

For transparency and SCRUM process we use task statuses:

- `backlog` - here we add all tasks from the idea, working on their context and description, decomposing them if needed before they goes to `todo`
- `todo` - tasks that are ready to be implemented or depends only on tasks in `todo` without obstacles and assigned to a developer
- `doing` - tasks in progress
- `review` - tasks that are ready and PR is waiting for review
- `test` - tasks deployed to `dev` or feature instance and waiting for QA or Business testing
- `done` - tasks that are tested and ready to be deployed to production

## 1. Developer creates feature branch (from `main` by default)

Naming convention: `[subproject/]feature_name`

Works in local environment.

## 2. Developer commits: `pre-commit` webhook

Pre-commit webhook runs tests and linters and stops commit if any test fails or lint error is found.

## 3. Developer pushes feature branch and creates pull request to `dev` or specific instance branch.

Every push triggers CI/CD pipeline that runs tests and linters and blocks merge to the next branch if any test fails or lint error is found.

## 4. Pull request review

Reviewer sees that CI/CD pipeline passed and can review the code and approve or reject the pull request.

## 5. Merge pull request

PR is merged to `dev` or specific instance branch. Then CI/CD pipeline runs tests and linters and blocks merge to the next branch if any test fails or lint error is found.

## 6. QA / Business testing

The code is deployed to `dev` or feature instance and QA or Business tests it. If any issues are found, the task is moved back to `todo` and the process repeats.

## 7. Merge request to main

Runs tests and linters and blocks merge to the next branch if any test fails or lint error is found.

## 8. Merge to main

Deploys to production: Blue/Green or Canary deployment.