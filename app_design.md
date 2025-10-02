# Task Manager MVP – Specification

## Purpose
A **personal task manager** (web-first, later mobile) with:
- Secure login (Google/Microsoft account)
- Private user data (no sharing yet)
- Tasks with metadata (priority, tags, due dates)
- Updates (free text, chronological timeline, hide toggle)
- Dashboard with latest update inline
- Weekly report generator (HTML body for email)

---

## Data Model

### User
- Authenticated via Google/Microsoft OAuth
- Each user sees only their own tasks/tags

### Task
- `id`
- `title` (required)
- `description`
- `targetDate`
- `priority` (Urgent, High, Medium, Low)
- `status` (New, In Progress, On Hold, Completed, Achieved, Cancelled, Deleted)
- `tags` (multiple, free text, reusable)
- `createdAt`
- `updatedAt`

### Update
- `id`
- `taskId` (linked to task)
- `text` (free form)
- `status` (optional, matches task status options)
- `createdAt`
- `hidden` (boolean)

### Tag
- `id`
- `name` (unique per user)
- Only deletable if unused

---

## Features

### Authentication
- Google + Microsoft OAuth (no passwords)

### Dashboard
- List of tasks (card/list style like Monday.com)
- Show **latest update under each task**
- Expand button → reveal full update history
- Filters: status, priority, tags, due date
- Quick actions: Complete, Hold, Cancel, Delete

### Task Details
- Task info (title, description, due date, tags, priority, status)
- Updates timeline (most recent first)
- Hide/unhide toggle for each update
- Add update form (free text + optional status change)

### Tags
- Autocomplete on typing, reuse existing tags, create if not found
- Tag management page → delete unused tags

### Weekly Report
- Button: “Generate Report”
- Output: HTML formatted list
- Sorted by **priority** (Urgent → High → Medium → Low)
- Show **last 3 updates per task**
- Exclude **Achieved** tasks

---

## Tech Stack

- **Frontend:** React + TailwindCSS
- **Backend/Database:** Firebase (Firestore + Auth)
- **Auth Providers:** Google, Microsoft
- **Deployment:** Firebase Hosting

---

## Firestore Structure

```
/users/{userId}/tasks/{taskId}
/users/{userId}/tasks/{taskId}/updates/{updateId}
/users/{userId}/tags/{tagId}
```

---

## Firestore Security Rules (draft)

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Each user only accesses their own data
    match /users/{userId}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

---

## Components

- `TaskCard` → Shows task info + latest update inline, expand button
- `TaskForm` → New/Edit form
- `UpdateTimeline` → Updates list with hide toggle
- `TagSelector` → Autocomplete input for tags
- `DashboardCharts` → Overview (priority/status)
- `ReportPreview` → Weekly report HTML view

---

## API Endpoints (if needed later)
- Auth handled by Firebase
- Firestore CRUD directly from client
- Report generator as client-side HTML builder

---

## Deliverables
- Firestore rules so each user only accesses their own data
- React components as listed
- Tailwind-based responsive UI similar to Monday.com
- Firebase configuration (`firebase.js`)
- Instructions for setup: `firebase init`, enabling Auth providers, Firestore setup
