# Wedding Client 💍

Frontend application for the **Wedding Platform**, providing a personalized digital wedding invitation experience and an administrative dashboard for managing guests, invitations, and RSVP responses.

## 1. Overview

`wedding-client` is a responsive web application built with **React and TypeScript**.

The application communicates with `wedding-server` through REST APIs and supports two main user experiences:

- **Wedding Guests** — View personalized invitations and confirm attendance.
- **Administrators** — Manage guests, invitations, WhatsApp messages, and RSVP statistics.

The application follows a feature-based architecture, separating business features from shared UI components and infrastructure.

## 2. Technology Stack

| Component | Technology |
|---|---|
| Framework | React |
| Language | TypeScript |
| Build Tool | Vite |
| Styling | Tailwind CSS |
| Routing | React Router |
| HTTP Client | Axios |
| Server State | TanStack Query |
| Forms | React Hook Form |
| Validation | Zod |
| Package Manager | npm |
| Development Environment | VS Code |

## 3. Application Architecture

```text
wedding-client/
├── public/
│   └── assets/
│
├── src/
│   ├── app/
│   │   ├── App.tsx
│   │   ├── router.tsx
│   │   └── providers.tsx
│   │
│   ├── features/
│   │   ├── wedding/
│   │   │   ├── components/
│   │   │   ├── pages/
│   │   │   └── hooks/
│   │   │
│   │   ├── invitation/
│   │   │   ├── components/
│   │   │   ├── pages/
│   │   │   ├── hooks/
│   │   │   └── api/
│   │   │
│   │   ├── rsvp/
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   ├── api/
│   │   │   └── schemas/
│   │   │
│   │   ├── guests/
│   │   │   ├── components/
│   │   │   ├── pages/
│   │   │   ├── hooks/
│   │   │   └── api/
│   │   │
│   │   ├── dashboard/
│   │   │   ├── components/
│   │   │   └── pages/
│   │   │
│   │   └── auth/
│   │       ├── components/
│   │       ├── pages/
│   │       └── hooks/
│   │
│   ├── shared/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── lib/
│   │   │   └── api-client.ts
│   │   ├── types/
│   │   └── utils/
│   │
│   ├── styles/
│   │   └── index.css
│   │
│   └── main.tsx
│
├── .env.example
├── index.html
├── package.json
├── tsconfig.json
├── vite.config.ts
└── README.md
```

The directory structure represents the intended architecture and can be introduced incrementally as features are implemented.

## 4. Core Features

### Wedding Website

The public wedding website provides a personalized experience for invited guests.

**Responsibilities:**
- Display the couple's names and wedding date.
- Display venue details and event schedule.
- Provide navigation links to the venue.
- Display a wedding countdown.
- Support responsive layouts for mobile and desktop.

### Invitation Management

Displays personalized invitation information based on a unique invitation token.

**Responsibilities:**
- Extract the invitation token from the URL.
- Retrieve invitation details from the backend.
- Display the invited guest's name.
- Handle invalid or expired invitation links.
- Display the guest's allowed attendee count.

### RSVP Management

Allows guests to respond to their invitations.

**Responsibilities:**
- Confirm or decline attendance.
- Select the number of attendees.
- Validate RSVP form inputs.
- Submit responses to the backend.
- Display confirmation and error states.
- Allow guests to update their responses.

### Guest Management

Provides administrators with tools for managing wedding guests.

**Responsibilities:**
- Display and search guest records.
- Add, edit, and delete guests.
- Import guest lists from CSV files.
- View invitation and RSVP statuses.
- Filter guests by attendance status.

### Admin Dashboard

Provides an overview of wedding attendance.

**Responsibilities:**
- Display the total number of invited guests.
- Display confirmed attendees.
- Display declined invitations.
- Display pending responses.
- Track invitation progress.

### WhatsApp Invitations

Provides an interface for sending personalized invitations.

**Responsibilities:**
- Display personalized invitation messages.
- Generate WhatsApp links for manual sending.
- Track manually recorded sending status.
- Trigger backend messaging operations when automated delivery is enabled.

## 5. Application Routes

### Public Routes

| Route | Description |
|---|---|
| `/` | Wedding landing page |
| `/invite/:token` | Personalized invitation page |
| `/invite/:token/rsvp` | RSVP form |
| `/invite/:token/confirmation` | RSVP confirmation |

### Admin Routes

| Route | Description |
|---|---|
| `/admin/login` | Administrator login |
| `/admin` | Dashboard overview |
| `/admin/guests` | Guest management |
| `/admin/guests/:id` | Guest details |
| `/admin/invitations` | Invitation management |
| `/admin/settings` | Wedding configuration |

Admin routes require authentication. The backend must independently enforce authorization on every administrative API.

## 6. Backend Integration

The frontend communicates with `wedding-server` using REST APIs.

### API Base URL

```env
VITE_API_BASE_URL=http://localhost:8080
```

### Example API Client

```typescript
import axios from "axios";

export const apiClient = axios.create({
  baseURL: import.meta.env.VITE_API_BASE_URL,
  timeout: 10000,
});
```

### Example Invitation Request

```typescript
import { apiClient } from "@/shared/lib/api-client";

export async function getInvitation(token: string) {
  const response = await apiClient.get(
    `/api/invitations/${encodeURIComponent(token)}`
  );

  return response.data;
}
```

All business validation and access control are enforced by the backend.

## 7. State Management

The application separates server state from local UI state.

**Server State — TanStack Query**
- Guest records.
- Invitation information.
- RSVP responses.
- Dashboard statistics.

**Local State — React**
- Form interactions.
- Modal visibility.
- Selected filters.
- Navigation state.

React Hook Form and Zod handle form state and client-side validation.

## 8. Authentication and Security

- Restrict administrative pages to authenticated users.
- Enforce administrative authorization in the backend.
- Avoid storing sensitive credentials in browser storage.
- Never expose server secrets in frontend environment variables.
- Validate form inputs on both frontend and backend.
- Handle invitation tokens carefully.
- Avoid logging personal guest information.
- Use HTTPS in production.

Frontend route protection improves user experience but does not replace backend security.

## 9. Local Development

### Prerequisites

- Node.js (current LTS version)
- npm
- VS Code

### Installation

From the `wedding-client` directory:

```bash
npm install
```

Create a `.env` file:

```env
VITE_API_BASE_URL=http://localhost:8080
```

Start the development server:

```bash
npm run dev
```

The application will typically be available at:

```text
http://localhost:5173
```

The backend should be running separately at:

```text
http://localhost:8080
```

## 10. Build and Deployment

### Production Build

```bash
npm run build
```

The production assets are generated in the `dist/` directory.

### Deployment

The application can be deployed to Vercel or another static hosting provider.

Recommended configuration:

- **Root Directory:** `wedding-client`
- **Framework Preset:** Vite
- **Build Command:** `npm run build`
- **Output Directory:** `dist`
- **Environment Variable:** `VITE_API_BASE_URL`

Configure SPA fallback routing so direct navigation to invitation and admin routes works correctly.

The frontend and backend are deployed independently from the same Git repository.

## 11. Testing Strategy

Recommended testing tools:

- **Vitest** — Unit tests.
- **React Testing Library** — Component tests.
- **Playwright** — End-to-end tests.

### Critical Test Scenarios

- Invitation page loads with a valid token.
- Invalid invitation links display an appropriate error.
- RSVP submission succeeds.
- Attendance count cannot exceed the invitation limit.
- API failures display meaningful feedback.
- Unauthorized users cannot access administrative functionality.
- WhatsApp invitation links contain the correct personalized URL.

## 12. Development Roadmap

### Phase 1 — Foundation
- Initialize React + TypeScript with Vite.
- Configure Tailwind CSS.
- Configure routing.
- Set up API client and shared components.

### Phase 2 — Public Wedding Website
- Build wedding landing page.
- Implement responsive invitation design.
- Display venue and wedding information.
- Build RSVP form.

### Phase 3 — Backend Integration
- Fetch personalized invitations.
- Submit RSVP responses.
- Handle loading and error states.

### Phase 4 — Admin Dashboard
- Implement admin authentication.
- Build guest management interface.
- Add guest search and filtering.
- Display RSVP statistics.

### Phase 5 — WhatsApp Integration
- Generate personalized WhatsApp messages.
- Support manual invitation sending.
- Integrate with backend messaging operations when available.

### Phase 6 — Production Release
- Deploy frontend.
- Configure production API URL.
- Test mobile responsiveness.
- Validate end-to-end invitation flows.
- Launch the application.

## 13. Future Enhancements

- Multilingual invitations.
- Wedding photo gallery.
- Guest grouping by family.
- Seating arrangement management.
- Automated RSVP reminders.
- QR-code invitations.
- Additional wedding themes.

---

**Project:** Wedding Platform  
**Application:** wedding-client  
**Architecture:** Feature-Based React Application  
**Language:** TypeScript  
**Framework:** React + Vite  
**Backend:** wedding-server (Spring Boot)
