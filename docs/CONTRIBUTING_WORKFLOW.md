# Skillix Contributing & Development Workflow

This document guides student developers, contributors, and maintainers on how to coordinate development between the **Frontend** and **Backend** teams, submit high-quality pull requests, and maintain project hygiene.

---

## 1. Team Coordination & Parallel Development

Because the platform is split into `frontend/` and `backend/` directories within a unified repository, members of both teams must work in harmony without blocking one another.

### 1.1 The API Contract as the Single Source of Truth
- **Before coding any new feature**, both teams must review and agree on the endpoint specification in [`docs/API_CONTRACT.md`](file:///Users/vikas/Desktop/skillix/docs/API_CONTRACT.md).
- **Backend Responsibility**: Implement the business logic, data models, validation schemas, and REST endpoints matching the agreed request and response envelope format.
- **Frontend Responsibility**: Build UI views, components, and client-side integration against mock data or the agreed endpoint shapes, without waiting for the backend to be fully deployed.

### 1.2 Issue Assignment & Tracking
1. Browse open issues on GitHub. Issues are tagged by domain:
   - `area/frontend` — Tasks for Next.js, React components, and Tailwind styling.
   - `area/backend` — Tasks for Express routes, MongoDB models, and Zod validation.
   - `area/docs` — Documentation and workflow improvements.
2. Comment on an issue expressing intent to work on it (e.g., *"I'd like to work on this issue"*).
3. Wait for maintainer assignment before starting to prevent duplicate or conflicting PRs.

---

## 2. Git Branching Strategy

All development branches are created from the latest `main` branch.

### 2.1 Branch Naming Conventions
Always use descriptive, kebab-case branch names prefixed with the change category:

| Prefix | Use Case | Example |
| :--- | :--- | :--- |
| `feat/` | New user-facing feature or API capability | `feat/frontend-event-card-component`, `feat/backend-event-model` |
| `fix/` | Bug fix in existing functionality | `fix/registration-duplicate-check`, `fix/navbar-mobile-toggle` |
| `docs/` | Documentation additions or corrections | `docs/update-api-contract-auth` |
| `refactor/`| Code refactoring without changing functionality | `refactor/zod-validation-pipeline` |
| `test/` | Adding or updating unit/integration tests | `test/backend-health-route` |
| `chore/` | Tooling, dependencies, or repository maintenance | `chore/setup-eslint-config` |

---

## 3. Commit Message Conventions

We strictly follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```text
<type>(<scope>): <short description in imperative mood>

[optional longer body explaining context and rationale]

[optional issue reference: Closes #123]
```

### Examples:
- `feat(frontend): create initial event card and grid layout`
- `feat(backend): introduce mongoose schema for event entity`
- `fix(backend): resolve validation error on event end date`
- `docs(api): document query parameters for event filtering`

---

## 4. Pull Request (PR) Lifecycle

### 4.1 Pre-PR Checklist
Before opening a pull request, ensure:
- [ ] Your local branch is rebased on the latest `origin/main`.
- [ ] Code follows formatting standards (e.g., ESLint, TypeScript strict mode once configured).
- [ ] Tests pass locally without errors.
- [ ] No extraneous files (e.g., `.DS_Store`, local `.env` with secrets, debug logs) are included.

### 4.2 Submitting the PR
1. Push your branch to your GitHub fork:
   ```bash
   git push origin <your-branch-name>
   ```
2. Navigate to the GitHub repository and click **Compare & pull request**.
3. Fill out the [Pull Request Template](file:///Users/vikas/Desktop/skillix/.github/PULL_REQUEST_TEMPLATE.md):
   - Summarize the change and motivation.
   - Link related issues (e.g., `Closes #42`).
   - Specify whether the change affects `frontend/`, `backend/`, or `docs/`.
4. Request reviews from the appropriate team lead or club maintainer.

### 4.3 Review & Merge Process
- At least **one approving review** from a project maintainer is required.
- CI automated checks must pass.
- Maintainers will squash and merge approved pull requests into `main`.
