# Skillix: System Architecture & Design

> **Notice**: This document outlines the **proposed architecture** for upcoming development. Neither the frontend nor backend applications are currently implemented. This serves as the blueprint for development teams.

## 1. High-Level Architecture Overview

Skillix is designed as a modular, decoupled full-stack application within a unified monorepo. It cleanly separates the client-side user experience from server-side domain logic and data persistence.

```text
┌────────────────────────────────────────────────────────┐
│                   Client Layer                         │
│   Next.js (App Router) + React 19 + Tailwind CSS       │
│   Reusable API Client (`src/lib/api-client.ts`)        │
└───────────────────────────┬────────────────────────────┘
                            │
                     HTTPS / JSON REST
               (Configurable CORS Boundary)
                            │
┌───────────────────────────▼────────────────────────────┐
│                    API Gateway Layer                   │
│   Express.js + CORS + Helmet + Body Parser             │
│   Request Validation Middleware (Zod Schemas)          │
│   Centralized Error Handling (`AppError`)              │
└───────────────────────────┬────────────────────────────┘
                            │
┌───────────────────────────▼────────────────────────────┐
│                  Domain Modules Layer                  │
│   ├── Events Module       (CRUD, Approvals, Feeds)     │
│   ├── Registrations Module (Capacity, Rosters)         │
│   ├── Opportunities Module (Curation, Discovery)       │
│   └── Applications Module  (Tracker Pipeline)          │
└───────────────────────────┬────────────────────────────┘
                            │
                     Mongoose ODM
                            │
┌───────────────────────────▼────────────────────────────┐
│                   Persistence Layer                    │
│   MongoDB Database (`skillix`)                         │
└────────────────────────────────────────────────────────┘
```

---

## 2. Layer Responsibilities

### 2.1 Frontend (`frontend/`)
- **Render UI & State**: Client components for interactive views and server/static components for landing and informational pages.
- **API Communication**: All network communication routes through `apiClient`, providing consistent request construction, header injection, and uniform error wrapping.
- **Routing**: Next.js App Router providing file-system routing.
- **Responsiveness & Theming**: Utility-first CSS via Tailwind CSS with dark and light theme token support.

### 2.2 Backend (`backend/`)
- **Request Ingress**: Express router with middleware chains for security headers, CORS origin enforcement, body parsing, and route logging.
- **Input Validation**: Zod schemas validate `req.body`, `req.query`, and `req.params` before controller execution.
- **Business Logic**: Domain services encapsulate rules (capacity constraints, duplicate registration checks, state transition guards).
- **Persistence**: Mongoose models handle schema enforcement, indexes, and document lifecycles.
- **Health & Telemetry**: `/api/health` reports system uptime, environment, and live database connection state.

---

## 3. Request-Response Lifecycle

```text
Client Request
      │
      ▼
1. CORS Middleware (Validates Origin against CLIENT_URL)
      │
      ▼
2. JSON Body Parser (Enforces payload size limits)
      │
      ▼
3. Request Validator (Validates input against Zod schema)
      ├── Invalid ──► Error Handler ──► Returns 400 with field issues
      └── Valid
            │
            ▼
4. Controller Handler (Extracts validated input, calls Service)
            │
            ▼
5. Domain Service (Executes business logic & rules)
            │
            ▼
6. Database Layer (Executes MongoDB operations via Mongoose)
            │
            ▼
7. Standard Response Formatter (`sendSuccess` / `sendError`)
            │
            ▼
Client Response (Uniform `{ success: true, data: ... }` JSON)
```

---

## 4. Module Boundaries & Domain Organization

To maintain clean separation of concerns and allow independent feature development, the backend organizes code into self-contained feature modules under `backend/src/modules/`:

| Module | Core Responsibility | Planned Components |
| :--- | :--- | :--- |
| **`events`** | Event lifecycle, organizer submissions, admin approvals, categories | `events.routes.ts`, `events.controller.ts`, `events.service.ts`, `events.model.ts`, `events.validation.ts` |
| **`registrations`** | Student registration, capacity limits, attendee rosters, check-in | `registrations.routes.ts`, `registrations.controller.ts`, `registrations.service.ts`, `registrations.model.ts` |
| **`opportunities`** | Verified external competitions, hackathons, scholarships | `opportunities.routes.ts`, `opportunities.controller.ts`, `opportunities.service.ts`, `opportunities.model.ts` |
| **`applications`** | Student application tracker stages (Saved, Applied, Interview) | `applications.routes.ts`, `applications.controller.ts`, `applications.service.ts`, `applications.model.ts` |

---

## 5. Database Integration & Data Modeling Strategy

- **Driver**: MongoDB with Mongoose ODM.
- **Connection Management**: Configured in `backend/src/db/connection.ts` with connection pooling, server selection timeout, and graceful teardown hooks on `SIGINT` / `SIGTERM`.
- **Integrity & Constraints**:
  - Event capacity checked atomically to prevent race conditions during concurrent registrations.
  - Compound unique index on `{ eventId: 1, studentId: 1 }` in the Registration collection to prevent duplicate registrations at the database level.
  - Foreign key-style references using MongoDB `ObjectId` with indexes on query fields (`status`, `startDate`, `deadline`).

---

## 6. Authentication & Authorization Strategy (Design Phase)

Authentication is planned before full feature implementation:

### Architecture Options Under Evaluation
1. **Option A: Session-based Auth with HTTP-only Cookies**
   - *Pros*: Highly secure against XSS attacks; straightforward revocation.
   - *Cons*: Requires server-side session store (e.g. MongoDB or Redis) if scaled horizontally.
2. **Option B: Stateless JWT with Refresh Tokens in HTTP-only Cookies**
   - *Pros*: Scales horizontally across serverless or containerized environments; low database overhead.
   - *Cons*: Token revocation requires a blocklist or short expiration windows.

### Role-Based Access Control (RBAC) Matrix
The application will enforce three distinct user roles:

| Role | Permissions |
| :--- | :--- |
| `student` | View approved events, register for events, view/cancel own registrations, bookmark opportunities, track personal applications. |
| `organizer` | All student permissions + create event drafts, edit owned drafts, view attendee rosters for owned events. |
| `admin` | All organizer permissions + approve/reject any pending event, curate/verify external opportunities, manage user roles. |

> **Security Rule**: Role verification is strictly enforced on the server within route middleware (`requireAuth`, `requireRole('admin')`). Frontend UI visibility controls are purely convenience features and never relied upon for access control.

---

## 7. Parallel Development Workflow

The decoupled architecture allows frontend and backend engineers to develop features concurrently:

1. **API First**: Before coding a module, engineers agree upon the endpoint contracts documented in `docs/API_CONTRACT.md`.
2. **Mocking & Isolation**:
   - The frontend uses the typed `apiClient` and TypeScript interfaces from `src/types/` to build UI features even before backend endpoints are deployed.
   - The backend uses Vitest and Supertest to write integration tests validating controllers against the contract without needing the frontend.
3. **Local Orchestration**: Root `npm run dev` boots both services via `concurrently`, providing hot reload across both layers.
