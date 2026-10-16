# Skillix: API Specification & Contract

> **Important**: All endpoints listed in this specification are **proposed, not implemented**. Neither the frontend nor backend application code has been created yet. This contract serves as the development blueprint for the backend and frontend teams.

This document defines the REST API contract for the Skillix backend. All requests and responses use JSON format with UTF-8 encoding.

## Response Envelope Conventions

### Success Response Format
```json
{
  "success": true,
  "message": "Optional human-readable confirmation",
  "data": { ... },
  "meta": {
    "timestamp": "2026-10-16T12:00:00.000Z"
  }
}
```

### Error Response Format
```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE_IDENTIFIER",
    "message": "Human-readable explanation of error",
    "details": [
      {
        "field": "title",
        "message": "Title must be at least 3 characters"
      }
    ]
  },
  "meta": {
    "timestamp": "2026-10-16T12:00:00.000Z"
  }
}
```

---

## Endpoint Status Legend
- :construction: **Proposed (Not Implemented)**: Architecture-approved target for future development.

---

## 1. System & Telemetry Endpoints

### 1.1 Health Check
- **Status**: :construction: **Proposed (Not Implemented)**
- **Method & Path**: `GET /api/health`
- **Auth Required**: None (Public)
- **Description**: Returns server status, uptime, environment, and MongoDB connection status.
- **Success Response (200 OK)**:
```json
{
  "success": true,
  "message": "System is operational",
  "data": {
    "status": "healthy",
    "service": "skillix-backend",
    "version": "0.1.0",
    "environment": "development",
    "uptimeSeconds": 128,
    "database": {
      "status": "connected",
      "readyState": 1
    }
  },
  "meta": {
    "timestamp": "2026-10-16T12:00:00.000Z"
  }
}
```

---

## 2. Event Management Endpoints (Initial MVP Workflow)

### 2.1 Submit Event Proposal
- **Status**: :construction: **Proposed**
- **Method & Path**: `POST /api/events`
- **Auth Required**: `organizer`
- **Description**: Club organizers submit an event proposal for admin review.
- **Request Body**:
```json
{
  "title": "HackCampus 2026",
  "description": "Annual 24-hour inter-college hackathon with prizes and mentor tracks.",
  "category": "hackathon",
  "venueType": "in-person",
  "venueDetails": "Auditorium Hall B",
  "startDate": "2026-11-15T09:00:00.000Z",
  "endDate": "2026-11-16T18:00:00.000Z",
  "registrationDeadline": "2026-11-10T23:59:59.000Z",
  "capacity": 150,
  "tags": ["coding", "hackathon", "web3"]
}
```
- **Success Response (201 Created)**:
```json
{
  "success": true,
  "message": "Event proposal submitted for admin review",
  "data": {
    "id": "67104b281f62d98a0c5f1122",
    "title": "HackCampus 2026",
    "status": "pending_approval",
    "createdAt": "2026-10-16T12:00:00.000Z"
  }
}
```

---

### 2.2 List Approved Events (Public Feed)
- **Status**: :construction: **Proposed**
- **Method & Path**: `GET /api/events`
- **Auth Required**: None (Public)
- **Query Parameters**:
  - `category` (optional): Filter by category (`hackathon`, `workshop`, `seminar`)
  - `page` (optional, default: 1): Page number
  - `limit` (optional, default: 10): Items per page
- **Success Response (200 OK)**:
```json
{
  "success": true,
  "data": {
    "events": [
      {
        "id": "67104b281f62d98a0c5f1122",
        "title": "HackCampus 2026",
        "category": "hackathon",
        "venueType": "in-person",
        "startDate": "2026-11-15T09:00:00.000Z",
        "endDate": "2026-11-16T18:00:00.000Z",
        "registrationDeadline": "2026-11-10T23:59:59.000Z",
        "capacity": 150,
        "registeredCount": 42,
        "status": "approved"
      }
    ],
    "pagination": {
      "total": 1,
      "page": 1,
      "totalPages": 1
    }
  }
}
```

---

### 2.3 Get Event Details
- **Status**: :construction: **Proposed**
- **Method & Path**: `GET /api/events/:id`
- **Auth Required**: None (Public for approved; Organizer/Admin for unapproved)
- **Success Response (200 OK)**:
```json
{
  "success": true,
  "data": {
    "id": "67104b281f62d98a0c5f1122",
    "title": "HackCampus 2026",
    "description": "Full event description...",
    "category": "hackathon",
    "venueType": "in-person",
    "venueDetails": "Auditorium Hall B",
    "startDate": "2026-11-15T09:00:00.000Z",
    "endDate": "2026-11-16T18:00:00.000Z",
    "registrationDeadline": "2026-11-10T23:59:59.000Z",
    "capacity": 150,
    "registeredCount": 42,
    "status": "approved",
    "tags": ["coding", "hackathon"]
  }
}
```

---

### 2.4 Admin Review Event (Approve / Reject)
- **Status**: :construction: **Proposed**
- **Method & Path**: `PATCH /api/events/:id/review`
- **Auth Required**: `admin`
- **Request Body**:
```json
{
  "action": "approve",
  "reviewNotes": "Approved by Campus Activities Council"
}
```
- **Success Response (200 OK)**:
```json
{
  "success": true,
  "message": "Event status updated to approved",
  "data": {
    "id": "67104b281f62d98a0c5f1122",
    "status": "approved",
    "reviewedAt": "2026-10-16T12:30:00.000Z"
  }
}
```

---

## 3. Registration Endpoints

### 3.1 Register for Event
- **Status**: :construction: **Proposed**
- **Method & Path**: `POST /api/events/:id/registrations`
- **Auth Required**: `student`
- **Description**: Registers the authenticated student for an approved event. Enforces capacity limits and duplicate prevention.
- **Success Response (201 Created)**:
```json
{
  "success": true,
  "message": "Successfully registered for HackCampus 2026",
  "data": {
    "registrationId": "67104c991f62d98a0c5f3344",
    "eventId": "67104b281f62d98a0c5f1122",
    "status": "confirmed",
    "registeredAt": "2026-10-16T12:45:00.000Z"
  }
}
```

---

### 3.2 List Event Registrations (Organizer View)
- **Status**: :construction: **Proposed**
- **Method & Path**: `GET /api/events/:id/registrations`
- **Auth Required**: `organizer` (event owner) or `admin`
- **Description**: Retrieve participant roster for an event.
- **Success Response (200 OK)**:
```json
{
  "success": true,
  "data": {
    "eventId": "67104b281f62d98a0c5f1122",
    "totalRegistrations": 42,
    "attendees": [
      {
        "registrationId": "67104c991f62d98a0c5f3344",
        "studentId": "std_101",
        "name": "Jane Doe",
        "email": "jane@campus.edu",
        "status": "confirmed",
        "registeredAt": "2026-10-16T12:45:00.000Z"
      }
    ]
  }
}
```

---

## 4. Error Codes & HTTP Status Codes

| Code | HTTP Status | Meaning |
| :--- | :--- | :--- |
| `VALIDATION_ERROR` | 400 | Request body, query, or params failed Zod schema parsing |
| `MALFORMED_JSON` | 400 | Request payload has syntax errors |
| `UNAUTHORIZED` | 401 | Missing, expired, or invalid authentication credentials |
| `FORBIDDEN` | 403 | User role has insufficient permissions for this operation |
| `NOT_FOUND` | 404 | Targeted resource or event ID does not exist |
| `ROUTE_NOT_FOUND` | 404 | API endpoint does not exist |
| `EVENT_CAPACITY_FULL` | 409 | Event has reached maximum registration capacity |
| `DUPLICATE_REGISTRATION` | 409 | Student is already registered for this event |
| `INTERNAL_SERVER_ERROR` | 500 | Unhandled server error |
