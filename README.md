# Facilities Desk

A campus facility work order system that lets students and staff report maintenance issues, and gives facilities staff a dashboard to track and resolve them.

---

## The Problem

Facility issues on campus — a broken light, a leaking pipe, a faulty AC unit — are usually reported informally: a conversation with a caretaker, a note left at an office, a message that gets lost. There's no ticket number, no status, and no record of what got fixed or when. Facilities Desk gives the campus a simple, structured way to capture and track that information.

## Overview

| | |
|---|---|
| **Type** | Web-based prototype (client-side only) |
| **Users** | Students/staff reporting issues, and facilities management staff |
| **Storage** | Browser `localStorage` — no backend or database required |
| **Stack** | HTML, CSS, vanilla JavaScript, Chart.js (via CDN) |

## How It Works

```
Student/Staff                      Facilities Staff
     │                                    │
     ▼                                    ▼
File a Report  ──────────────────►  Sign In (Admin/Staff)
(file-report.html)                  (login.html)
     │                                    │
     ▼                                    ▼
Ticket saved to browser storage  ──►  Ticket Board (admin.html)
(auto-generated ID, e.g. WO-0001)   Filter, search, sort tickets
                                     Open ticket → advance status
                                     Open → Acknowledged → In
                                     Progress → Resolved
                                            │
                                            ▼
                                     Dashboard: stats & charts
```

All data lives in the browser's `localStorage` under the key `facilitiesIssues`, so it persists across sessions on the same device without needing a server.

## File Structure

| File | Purpose |
|---|---|
| `index.html` | Landing page — routes to "File a Report" or "Management" |
| `file-report.html` | Reporting form used by students/staff to submit a work order |
| `login.html` | Staff sign-in (demo login — any credentials, or one-click demo buttons) |
| `admin.html` | Management dashboard: ticket board, filters, ticket detail view, analytics |
| `facilities-desk.html` | Single-file version of the same system (portable/backup copy) |

## Getting Started

**Option A — Open directly**
1. Download the files into a folder.
2. Open `index.html` in a browser (Chrome, Firefox, Edge, or Safari).

**Option B — Local server**
```bash
# Python
python3 -m http.server 8000

# Node.js
npx http-server
```
Then visit `http://localhost:8000`.

No installation, build step, or dependencies are required beyond an internet connection (for the Google Fonts and Chart.js CDN links).

## Features

**Reporting (student/staff facing)**
- Category (Electrical, Plumbing, Furniture, IT & Wi-Fi, HVAC, Structural, Cleaning, Other)
- Urgency level (Low / Medium / High)
- Building (8 preset campus buildings) and room/location
- Detailed description field
- Optional: reporter name, contact info, photo upload, anonymous reporting
- Automatic ticket ID on submission

**Management (staff facing)**
- Ticket board with search and filters (category, building, status, urgency) and sorting (newest, oldest, most urgent)
- Ticket detail view with full description, contact info, and attached photo
- Status workflow: Open → Acknowledged → In Progress → Resolved, with resolution notes
- Dashboard tab with summary stats and charts (issues by category, status breakdown)
- Demo login (Admin / Staff roles) — no real authentication in this prototype

## Data Model (summary)

Each ticket stored in `localStorage["facilitiesIssues"]` includes:

- `id` — ticket number (e.g. `WO-0001`)
- `category`, `urgency`, `building`, `room`, `description`
- `reporterName`, `contact`, `anonymous`, `photo` (optional)
- `status` — Open / Acknowledged / In Progress / Resolved
- `dateReported`, status timestamps, `resolutionNote`

## Known Limitations (Prototype Scope)

- **No real backend** — data is local to the browser/device and not shared across users or devices.
- **No real authentication** — the login page demonstrates the flow but does not validate credentials.
- **No notifications** — status changes are not emailed or messaged to reporters.

## Path to Production

| Area | Prototype | Production |
|---|---|---|
| Data storage | Browser `localStorage` | PostgreSQL (or similar) + REST API backend |
| Authentication | Demo login | University SSO (SAML/CAS) or proper credential backend |
| Notifications | None | Email/SMS alerts on status change |
| Access | Single device | Multi-user, real-time shared board |

## Reset Demo Data

The system loads with 12 sample tickets on first use. To reset:
1. Open browser DevTools (F12) → Application → Local Storage
2. Delete the `facilitiesIssues` key
3. Refresh `admin.html`

---

*Facilities Desk — a lightweight prototype for campus maintenance reporting and tracking.*
