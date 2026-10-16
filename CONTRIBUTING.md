# Contributing to Skillix

Welcome to **Skillix (Unified Event & Opportunity Management Platform)**! We are an open-source campus project developed by and for college students, mentors, and club organizers.

> **Project Phase: Repository Foundation & Initial Skeleton**
>
> Implementation of the frontend and backend applications has **not started yet**. This repository currently contains the directory skeleton, architectural blueprints, and API contracts. Development will begin from scratch according to the guidelines below.

---

## Quick Reference

- **Full Workflow Guide**: See [`docs/CONTRIBUTING_WORKFLOW.md`](file:///Users/vikas/Desktop/skillix/docs/CONTRIBUTING_WORKFLOW.md) for detailed instructions on team coordination and parallel development.
- **Code of Conduct**: All contributors must adhere to our [`CODE_OF_CONDUCT.md`](file:///Users/vikas/Desktop/skillix/CODE_OF_CONDUCT.md).
- **Security Policy**: See [`SECURITY.md`](file:///Users/vikas/Desktop/skillix/SECURITY.md) for responsible disclosure.
- **Project Scope**: See [`docs/PROJECT_SCOPE.md`](file:///Users/vikas/Desktop/skillix/docs/PROJECT_SCOPE.md).
- **Architecture**: See [`docs/ARCHITECTURE.md`](file:///Users/vikas/Desktop/skillix/docs/ARCHITECTURE.md).
- **API Contract**: See [`docs/API_CONTRACT.md`](file:///Users/vikas/Desktop/skillix/docs/API_CONTRACT.md).

---

## 1. How to Get Involved

1. **Check Open Issues**: Look for issues tagged `good first issue`, `area/frontend`, `area/backend`, or `documentation`.
2. **Claim an Issue**: Comment on the issue you wish to work on before starting. A maintainer will assign it to you.
3. **Propose New Ideas**: If you want to propose a new feature, open an issue using the [Feature Request template](file:///Users/vikas/Desktop/skillix/.github/ISSUE_TEMPLATE/feature_request.md) for discussion.

---

## 2. Development Setup & Starting from Scratch

When implementation begins:
- **Frontend Team**: Will initialize the Next.js App Router project inside [`frontend/`](file:///Users/vikas/Desktop/skillix/frontend) using React, TypeScript, and Tailwind CSS.
- **Backend Team**: Will initialize the Express.js project inside [`backend/`](file:///Users/vikas/Desktop/skillix/backend) using Node.js, TypeScript, Mongoose, and Zod.

Do not commit application code outside of your assigned module. Always ensure the API contract in [`docs/API_CONTRACT.md`](file:///Users/vikas/Desktop/skillix/docs/API_CONTRACT.md) is respected.

---

## 3. Git Branching & Commits

- Branch from `main`:
  ```bash
  git checkout -b <type>/<descriptive-name>
  ```
  Examples: `feat/event-creation-form`, `feat/express-server-init`, `docs/update-scope`

- Use [Conventional Commits](https://www.conventionalcommits.org/):
  - `feat`: New feature or API capability
  - `fix`: Bug fix
  - `docs`: Documentation updates
  - `refactor`: Code refactoring without behavior changes
  - `test`: Adding or updating tests
  - `chore`: Tooling, dependency, or config changes

---

## 4. Submitting Pull Requests

1. Push your branch to your fork.
2. Open a Pull Request targeting `main`.
3. Complete the [Pull Request Template](file:///Users/vikas/Desktop/skillix/.github/PULL_REQUEST_TEMPLATE.md).
4. Ensure all CI checks pass.
5. Address review comments respectfully. Once approved by a maintainer, your PR will be merged.
