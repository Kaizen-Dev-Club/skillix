# Skillix: Unified Event & Opportunity Management Platform

An open-source campus platform designed to help college students discover opportunities and manage event participation, while providing student organizers and administrators with tools to publish, approve, and manage campus activities.

---

> ### ⚠️ Project Status: Repository Skeleton & Documentation Only
> **Application implementation has not started.**
>
> This repository contains the directory structure, architectural blueprints, API contracts, and contributor guidelines. No frontend components, backend servers, database models, or dependencies are currently installed. Club members and contributors will develop the frontend and backend from scratch using this documentation as their primary reference.

---

## 1. Project Vision & Purpose

### The Problem
On college campuses, event details and competitive opportunities are fragmented across WhatsApp groups, Discord servers, social media stories, physical bulletin boards, and unstructured mailing lists. Consequently:
- **Students** miss critical registration deadlines for hackathons, workshops, scholarships, and internships.
- **Club Organizers** rely on manual Google Forms and disconnected spreadsheets, leading to capacity oversights and inefficient check-in processes.
- **College Administrators** lack visibility into student club activities and have no unified channel for review, moderation, and quality control.

### The Vision
**Skillix** provides a single, verified campus ecosystem where:
1. Student club organizers submit structured event proposals for administrative approval.
2. College administrators review and moderate campus activities through a centralized workflow.
3. Students discover verified campus events and high-impact external opportunities in one curated feed.
4. Students track event RSVPs and monitor application pipeline stages on a personal dashboard.

---

## 2. Intended Users & Stakeholders

| User Persona | Role & Core Needs |
| :--- | :--- |
| **College Student** | Discovers verified campus events, registers with real-time capacity checks, explores external hackathons and internships, and tracks personal application stages. |
| **Club Organizer** | Submits event proposals, monitors attendee registrations against capacity limits, and manages attendee rosters. |
| **Campus Administrator** | Moderates event proposals, validates campus policy compliance, and approves/rejects submissions with review feedback. |
| **Opportunity Curator** | Reviews and verifies external opportunities (hackathons, competitions, internships) to maintain a trustworthy directory. |

---

## 3. Core Modules & Responsibilities

The platform is architected around three interconnected domain modules:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        Skillix Core Modules                            │
├─────────────────────┬───────────────────────────┬──────────────────────┤
│ 1. Event Management │ 2. Opportunity Discovery  │ 3. Student Dashboard │
├─────────────────────┼───────────────────────────┼──────────────────────┤
│ • Event submission  │ • Verified hackathons     │ • Registered events  │
│ • Admin approvals   │ • Internship listings     │ • Bookmarked items   │
│ • Student RSVP      │ • Scholarship discovery   │ • Pipeline stages    │
│ • Attendance roster │ • Manual curation checks  │ • Application notes  │
└─────────────────────┴───────────────────────────┴──────────────────────┘
```

1. **Event Management (MVP Focus)**: End-to-end management of campus events from club submission through administrative review, public discovery, student registration, and organizer roster management.
2. **Opportunity Discovery (Phase 2)**: A curated directory of verified external hackathons, coding contests, internships, and fellowships with eligibility details and deadline countdowns.
3. **Student Dashboard (Phase 2)**: A personal student tracker for registered campus events, saved opportunities, application status pipelines (*Saved*, *Applied*, *Interviewing*, *Offered*, *Rejected*), and personal notes.

---

## 4. MVP Scope & Acceptance Criteria

### MVP Workflow
The Phase 1 MVP focuses on validating the fundamental college event lifecycle on a single pilot campus:

```text
[1. Organizer]        [2. Admin]           [3. Student]        [4. Student]        [5. Organizer]
Create Event   ───►  Review & Approve ───►  Discover Event ───►  Register for ───►  Manage Attendee
Proposal             Proposal               in Feed              Event                Roster
(Status: Pending)    (Status: Approved)     (Filtered view)      (Capacity checked)   (View attendees)
```

### Acceptance Criteria
To declare the Phase 1 MVP complete:
1. **Event Creation**: An authorized organizer can create an event proposal saved with status `pending_approval` (hidden from public feed).
2. **Admin Review**: An administrator can approve or reject pending events with optional feedback notes.
3. **Event Discovery**: Only approved events appear in the student feed, sorted by date and filterable by category.
4. **Registration**: Students can register for approved events; the system strictly enforces capacity limits and prevents duplicate registrations.
5. **Attendee Roster**: Organizers can view participant rosters for their own events.
6. **System Health**: A backend health-check route reports server status and database connectivity.

---

## 5. Features Explicitly Excluded from MVP

To maintain focus on a rock-solid foundation, the following features are **explicitly deferred**:
- **Automated Web Scraping**: External opportunities will initially be curated and verified manually.
- **AI/ML Recommendations**: No algorithmic matchmaking or automated recommendations in Phase 1.
- **Payment Processing**: No ticketing fees or paid registrations in the initial scope.
- **Multi-University Federation**: Architecture supports future multi-tenancy, but implementation focuses on a single campus first.
- **Native Mobile Apps**: Native iOS/Android apps are deferred in favor of a responsive web experience.
- **Third-Party Email/SMS Notifications**: External notification integrations are deferred until core workflows are proven.

---

## 6. Planned Technology Stack

| Layer | Technology | Rationale |
| :--- | :--- | :--- |
| **Frontend** | Next.js (React, App Router) | Fast server-side rendering, static generation, file-system routing, and a modern developer experience. |
| **Styling** | Tailwind CSS | Utility-first styling enabling responsive layouts and design system consistency without runtime CSS overhead. |
| **Backend** | Node.js + Express.js | Lightweight, mature, unopinionated framework well suited for modular REST APIs. |
| **Language** | TypeScript (Strict) | End-to-end type safety between client and server domain models to prevent runtime contract errors. |
| **Database** | MongoDB + Mongoose ODM | Flexible document modeling for rich event metadata, tags, and attendee lists. |
| **Validation** | Zod | Declarative, type-inferred schema validation for request bodies, query params, and environment variables. |

---

## 7. Responsibilities of Frontend & Backend Teams

### Frontend Team Responsibilities (`frontend/`)
- Initialize Next.js project with TypeScript and Tailwind CSS inside `frontend/`.
- Build reusable UI primitives (`Button`, `Card`, `Badge`) and layout components (`Navbar`, `Footer`).
- Implement pages for event browsing, event detail views, submission forms, and student registration.
- Build a typed API client adhering to [`docs/API_CONTRACT.md`](file:///Users/vikas/Desktop/skillix/docs/API_CONTRACT.md).
- Develop client-side state management and error displays without relying on hidden controls for security.

### Backend Team Responsibilities (`backend/`)
- Initialize Express server with TypeScript, CORS, and centralized error handling inside `backend/`.
- Configure Mongoose connection and lifecycle hooks in `backend/src/db/`.
- Validate all incoming requests using Zod schemas before passing them to service handlers.
- Enforce business logic: atomic capacity checking, duplicate registration prevention, and RBAC rules.
- Implement the health check endpoint and domain routes according to [`docs/API_CONTRACT.md`](file:///Users/vikas/Desktop/skillix/docs/API_CONTRACT.md).

---

## 8. Repository Folder Structure

```text
skillix/
├── frontend/                      # Frontend application workspace (to be implemented)
│   ├── public/                    # Static assets (images, icons, favicons)
│   ├── src/
│   │   ├── app/                   # Next.js App Router (pages and layouts)
│   │   ├── components/            # Reusable UI primitives and layout shells
│   │   ├── features/              # Feature modules (events, opportunities, dashboard)
│   │   ├── lib/                   # API client and client-side utilities
│   │   └── types/                 # Shared TypeScript interfaces
│   └── tests/                     # Frontend unit and component tests
├── backend/                       # Backend service workspace (to be implemented)
│   ├── src/
│   │   ├── config/                # Environment configuration and validation
│   │   ├── db/                    # Database connection and lifecycle management
│   │   ├── middleware/            # Error handling, request validation, CORS, auth
│   │   ├── modules/               # Domain feature modules
│   │   │   ├── events/            # Event creation, review, and public listings
│   │   │   ├── registrations/     # Registration handling and capacity enforcement
│   │   │   ├── opportunities/     # Opportunity curation and discovery
│   │   │   └── applications/      # Student application tracker
│   │   ├── routes/                # API route definitions
│   │   └── utils/                 # Response formatters and AppError helpers
│   └── tests/                     # Backend integration and unit tests
├── docs/                          # Comprehensive project documentation
│   ├── PROJECT_SCOPE.md           # Scope boundaries, user personas, acceptance criteria
│   ├── ARCHITECTURE.md            # System architecture, layer responsibilities, RBAC
│   ├── API_CONTRACT.md            # Proposed REST API endpoint specifications
│   └── CONTRIBUTING_WORKFLOW.md   # Team coordination, branching, and PR lifecycle
├── .github/                       # GitHub community templates and CI workflows
│   ├── workflows/
│   │   └── ci.yml                 # Automated CI workflow
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md          # Bug report template
│   │   └── feature_request.md     # Feature request template
│   └── PULL_REQUEST_TEMPLATE.md   # Pull request review checklist
├── .gitignore                     # Git exclusions for dependencies, builds, and secrets
├── CONTRIBUTING.md                # Contributor guide and quickstart
├── CODE_OF_CONDUCT.md             # Contributor Covenant Code of Conduct
├── SECURITY.md                    # Responsible vulnerability disclosure policy
├── LICENSE                        # MIT License
└── README.md                      # Project master documentation (this file)
```

---

## 9. API Contract & Integration Expectations

All client-server communication will follow standard JSON REST conventions as detailed in [`docs/API_CONTRACT.md`](file:///Users/vikas/Desktop/skillix/docs/API_CONTRACT.md).

- **Standard Success Format**:
  ```json
  {
    "success": true,
    "message": "Optional confirmation message",
    "data": { ... },
    "meta": { "timestamp": "ISO-8601" }
  }
  ```
- **Standard Error Format**:
  ```json
  {
    "success": false,
    "error": {
      "code": "ERROR_CODE",
      "message": "Human-readable description",
      "details": [ ... ]
    },
    "meta": { "timestamp": "ISO-8601" }
  }
  ```
- All endpoints documented in `docs/API_CONTRACT.md` are **proposed targets** for future implementation.

---

## 10. Contributing, Branching & Issue Workflow

We welcome contributions from students, developers, and club members.

1. **Find or Open an Issue**: Browse open issues on GitHub or propose a new task via an issue template.
2. **Branch Naming**: Create a branch off `main` with a descriptive prefix:
   - `feat/<feature-name>` (e.g., `feat/event-creation-form`)
   - `fix/<bug-name>` (e.g., `fix/registration-duplicate-check`)
   - `docs/<doc-name>` (e.g., `docs/api-contract-update`)
3. **Commit Messages**: Follow [Conventional Commits](https://www.conventionalcommits.org/):
   - `feat(events): add event proposal form`
   - `fix(auth): correct token expiration handling`
4. **Pull Requests**: Open a PR filling out the [Pull Request Template](file:///Users/vikas/Desktop/skillix/.github/PULL_REQUEST_TEMPLATE.md). All PRs require review from a maintainer and passing CI checks.
5. For full guidelines, read [`docs/CONTRIBUTING_WORKFLOW.md`](file:///Users/vikas/Desktop/skillix/docs/CONTRIBUTING_WORKFLOW.md) and [`CONTRIBUTING.md`](file:///Users/vikas/Desktop/skillix/CONTRIBUTING.md).

---

## 11. Testing & Security Expectations

When implementation begins, contributors must adhere to these standards:

### Testing Expectations
- **Backend**: Every endpoint must have automated tests validating successful responses, input validation failures (400), authentication failures (401/403), and resource conflicts (409).
- **Frontend**: Key components and API client methods must have unit tests covering loading, success, and error states.
- **Pre-commit**: Contributors must run linting, type checks, and tests before opening a pull request.

### Security Expectations
- **Never Commit Secrets**: Environment variables (`.env`) containing real secrets or database URIs must never be committed. Templates (`.env.example`) must be used.
- **Input Validation**: All incoming requests must be parsed and validated with Zod before processing.
- **Server-Side Authorization**: Hiding a button on the frontend is not security. All state changes and sensitive operations must be verified and authorized on the backend.
- **Responsible Disclosure**: See [`SECURITY.md`](file:///Users/vikas/Desktop/skillix/SECURITY.md) for vulnerability reporting procedures.

---

## 12. Future Roadmap

- **Phase 1 (Current Target)**: Core campus event management workflow (Organizer submission → Admin approval → Student discovery → Registration → Attendee management).
- **Phase 2 (Planned)**: Opportunity discovery catalog (verified hackathons, internships, scholarships) and personalized student dashboard.
- **Phase 3 (Future Consideration)**: Multi-college tenant federation, push notifications, and attendee QR check-in capabilities.

---

## 13. License & Community

- **License**: This project is licensed under the [MIT License](file:///Users/vikas/Desktop/skillix/LICENSE).
- **Code of Conduct**: We are committed to an open, inclusive, and harassment-free community. See [`CODE_OF_CONDUCT.md`](file:///Users/vikas/Desktop/skillix/CODE_OF_CONDUCT.md).
- **Community**: Maintained with passion by the **[Kaizen Dev Club](https://github.com/Kaizen-Dev-Club)**.