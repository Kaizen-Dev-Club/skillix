# Skillix: Project Scope & Requirements

> **Current Status**: Foundation / Skeleton Phase. Implementation of the frontend and backend applications has **not started yet**. This document defines the problem statement, personas, scope boundaries, and acceptance criteria for upcoming development.

## 1. Problem Statement

College students often struggle to discover campus events, workshops, technical competitions, hackathons, and career opportunities because information is fragmented across informal social media groups, physical posters, and disjointed mailing lists. At the same time, student club organizers face manual and error-prone processes for managing event proposals, administrative approvals, and attendee registrations.

**Skillix (Unified Event & Opportunity Management Platform)** addresses this gap by creating an integrated, verified campus platform where:
1. Student organizers can publish and manage campus events with administrative oversight.
2. Students can discover verified events and external opportunities in one centralized location.
3. Students can manage their registrations and track application milestones through a dedicated personal dashboard.

---

## 2. Target Users & Stakeholders

| User Persona | Role & Core Needs |
| :--- | :--- |
| **College Student** | Discovers verified campus events, registers with a single click, explores external hackathons and internships, and tracks participation. |
| **Club Organizer** | Submits campus event proposals, tracks registration counts against capacity, and manages attendee check-in rosters. |
| **Campus Administrator** | Reviews and moderates club event submissions, enforces campus guidelines, and maintains quality assurance. |
| **Opportunity Curator** | Manages verified external competitions, fellowships, and scholarships with deadlines and eligibility criteria. |

---

## 3. Phased Roadmap & Scope Boundaries

### 3.1 Initial MVP Scope (Phase 1 — Current Work)

The initial MVP focuses exclusively on validating the **core college event lifecycle** within a single pilot campus:

```text
[Organizer] Creates Event Proposal
          ↓
[Admin] Reviews, Validates & Approves Event
          ↓
[Student] Discovers Approved Event in Campus Feed
          ↓
[Student] Registers for the Event
          ↓
[Organizer] Views & Manages Attendee Registration Roster
```

#### In-Scope for Phase 1 MVP:
- **Event Management**:
  - Event submission with title, description, category, venue, dates, capacity, and tags.
  - Admin approval workflow (Draft → Pending Approval → Approved / Rejected).
  - Public listing and detail view of approved events.
- **Registration Management**:
  - Student registration for approved events.
  - Capacity enforcement (preventing registrations beyond event limits).
  - Duplicate registration prevention (one registration per student per event).
  - Organizer registration roster view.
- **Core Infrastructure**:
  - Express API with Zod request validation and error middleware.
  - Next.js frontend with Tailwind CSS and responsive UI.
  - MongoDB database schemas for Events and Registrations.
  - System health monitor endpoint (`/api/health`).

#### Out-of-Scope for Phase 1 MVP (Deferred to Later Phases):
- Automated web scrapers for external competitions.
- AI/ML recommendation engines or smart matchmaking.
- Payment gateways or ticketing monetization.
- Multi-tenant multi-university federation.
- Push notifications, SMS alerts, or third-party email providers (SendGrid/Mailgun).
- QR code attendance scanner check-in app.
- Mobile native applications (iOS/Android).

---

### 3.2 Phase 2 Scope (Planned)

Once the core event workflow is stable:
1. **Opportunity Discovery**:
   - Manually curated directory of verified hackathons, internships, scholarships, and fellowships.
   - Tag-based filtering (e.g., `#webdev`, `#ai`, `#freshmen-eligible`).
   - External application links and application deadline countdowns.
2. **Student Dashboard**:
   - Registered events timeline and upcoming reminders.
   - Saved external opportunities directory.
   - Personal application tracking pipeline (Stages: *Saved*, *Applied*, *Interviewing*, *Offered*, *Rejected*).
   - Personal application notes.

---

## 4. MVP Acceptance Criteria

To declare the Phase 1 MVP complete, the platform must satisfy the following verifiable criteria:

1. **Event Creation**:
   - An authorized club organizer can create an event with required metadata.
   - The event is saved with status `pending_approval` and is not visible in public listings.

2. **Admin Review**:
   - An administrator can review the pending queue and transition an event to `approved` or `rejected` with review feedback.

3. **Event Discovery**:
   - Approved events appear in the student event feed sorted by start date.
   - Unapproved or rejected events are excluded from the public feed.

4. **Event Registration**:
   - A student can register for an approved event.
   - The system rejects registration attempts if the event capacity is reached or if the student is already registered.

5. **Organizer Roster**:
   - The event organizer can query and view all registered attendees for their events.

6. **System Reliability**:
   - The backend exposes a working `/api/health` check endpoint.
   - All input is validated using Zod, returning standard 400 Bad Request responses with detailed field errors on invalid payloads.
