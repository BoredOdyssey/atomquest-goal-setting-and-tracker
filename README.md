# atomquest-goal-setting-and-tracker
# AtomQuest – Goal Setting & Tracking Portal

AtomQuest is a lightweight, browser-based Goal Setting & Performance Tracking Portal built for Hackathon 1.0. The application provides employees, managers, and administrators with a complete workflow for goal creation, approvals, quarterly check-ins, analytics, escalation monitoring, and reporting — all without requiring a backend server or database.

The entire system runs as a single HTML file using Vanilla JavaScript, HTML5, CSS3, and browser localStorage.
Working link - https://zesty-sprinkles-723f14.netlify.app/
---

#  Features

##  Role-Based Access

### Employee

* Create and manage goals
* Submit goals for approval
* Log quarterly achievements
* Track progress and scores
* View dashboards and check-ins

###  Manager

* Review employee goals
* Approve or return goals
* Conduct quarterly check-ins
* Monitor team performance
* Access team dashboards

###  Admin / HR

* View organization-wide dashboards
* Push shared departmental goals
* Run escalation scans
* View analytics and reports
* Export CSV reports
* Access audit logs

---

#  Core Modules

##  Goal Management

* Create goals with:

  * Title
  * Description
  * Thrust area
  * Target value
  * Weightage
  * Unit of Measurement (UoM)

### Validation Rules

* Maximum 8 goals per employee
* Minimum 10% weightage per goal
* Total weightage must equal 100%
* Goal locking after approval
* Role-based access enforcement

---

##  Quarterly Check-ins

* Q1 – Q4 check-in windows
* Achievement logging
* Progress tracking
* Manager comments and reviews
* Performance scoring

---

##  Analytics Dashboard

* Goal status distribution
* Quarter-on-quarter trends
* Goal heatmaps
* Manager effectiveness tracking
* Department analytics
* Canvas-based charts

---

##  Escalation Engine

Tracks:

* Employees who did not submit goals
* Goals pending approval
* Missed manager check-ins
* Escalation logs
* Rule-based monitoring

---

##  Audit Logging

Every important action is recorded:

* Goal creation
* Goal edits
* Approvals
* Returns
* Unlocks
* Achievement updates
* Check-ins

---

#  Architecture Overview

## Presentation Layer

* HTML5
* CSS3
* Responsive UI
* Single-page application

## Application Logic Layer

Built completely using Vanilla JavaScript:

* Goal module
* Check-in module
* Escalation module
* Analytics module
* Validation engine

## Data Layer

Uses browser localStorage for persistence.

Stored keys include:

* aq_users
* aq_goals
* aq_achievements
* aq_checkins
* aq_audit
* aq_session
* aq_next_id
* aq_esclog

---

#  Technology Stack

| Component       | Technology                        |
| --------------- | --------------------------------- |
| Frontend        | HTML5 + CSS3 + Vanilla JavaScript |
| Charts          | HTML5 Canvas API                  |
| Database        | Browser localStorage              |
| Authentication  | Session object in localStorage    |
| Hosting         | GitHub Pages / Netlify / Vercel   |
| Export          | Blob API → CSV                    |
| Version Control | Git + GitHub                      |

---

#  Data Flow

## Login Flow

User enters credentials → JS validates against localStorage → session created → role-specific UI rendered.

## Goal Creation

Employee creates goal → validation rules applied → goal stored → audit entry generated.

## Goal Approval

Manager reviews goal → approves or returns → goal status updated → audit recorded.

## Achievement Logging

Employee logs actual achievement → score calculated automatically → achievement saved.

## Escalation Scan

Admin runs scan → rules executed → violations detected → escalation log updated.

---

#  Scoring Logic

The system supports 4 Unit of Measurement (UoM) types:

| UoM Type                   | Description                                  |
| -------------------------- | -------------------------------------------- |
| Numeric – Higher is Better | Score increases as actual exceeds target     |
| Numeric – Lower is Better  | Lower actual value gives better score        |
| Timeline                   | Based on completion before/after target date |
| Zero-based                 | 100% only if actual = 0                      |

---

#  Authentication

Demo authentication is implemented using localStorage sessions.

## Demo Accounts

| Role     | Username | Password   |
| -------- | -------- | ---------- |
| Admin    | admin    | admin123   |
| Manager  | manager1 | manager123 |
| Employee | emp1     | emp123     |
| Employee | emp2     | emp123     |

---

#  Project Structure

```text
AtomQuest/
│
├── atomquest.html        # Main application
├── architecture.html     # Architecture diagram and documentation
└── README.md             # Project documentation
```

---

#  How to Run

## Option 1 — Run Locally

1. Download the project files
2. Open `atomquest.html` in any browser
3. Start using the application

No installation or setup required.

---

## Option 2 — GitHub Pages

1. Create a GitHub repository
2. Upload the files
3. Enable GitHub Pages
4. Open the generated live URL

---

## Option 3 — Netlify / Vercel

1. Drag and drop the project folder
2. Deployment happens instantly
3. Access live hosted version

---

#  Browser Compatibility

Supported browsers:

* Google Chrome
* Mozilla Firefox
* Microsoft Edge
* Safari

---

#  Responsive Design

The application supports:

* Desktop screens
* Tablets
* Mobile browsers

---

#  Key Highlights

* Completely free to build and host
* No backend required
* No external dependencies
* Offline-capable after first load
* Single-file architecture
* Easy deployment
* Fast performance
* Clean responsive UI

---

#  Future Improvements

Possible enhancements:

* Firebase / Supabase integration
* Real authentication system
* Cloud database
* Email notifications
* AI-driven analytics
* PDF report export
* Team chat and collaboration
* Multi-organization support

---

# Testing Checklist

## Employee Testing

* Goal creation
* Goal submission
* Achievement logging
* Dashboard rendering

## Manager Testing

* Goal approvals
* Goal returns
* Check-in creation
* Team monitoring

## Admin Testing

* Shared goal push
* Analytics rendering
* Escalation scan
* CSV export

---

#  Business Rules

* Maximum 8 goals per employee
* Minimum 10% weightage per goal
* Total goal weightage must equal 100%
* Locked goals cannot be edited
* Quarterly windows enforced
* Managers can return goals for revision
* Shared goals are partially editable

---

#  Analytics Included

* Goal completion tracking
* Department performance
* Quarterly score comparison
* Goal distribution
* Manager effectiveness
* Escalation monitoring

---

#  Quarterly Windows

| Quarter | Window             |
| ------- | ------------------ |
| Q1      | July – September   |
| Q2      | October – December |
| Q3      | January – March    |
| Q4      | April – June       |

Goal-setting window opens from May onwards.

---

# Deployment Cost

| Service      | Cost      |
| ------------ | --------- |
| HTML/CSS/JS  | Free      |
| localStorage | Free      |
| GitHub Pages | Free      |
| Netlify      | Free Tier |
| Vercel       | Free Tier |

Total deployment cost: ₹0

---

#  Development Approach

The project follows:

* Client-side architecture
* Single-page application design
* Modular JavaScript structure
* Browser-native persistence
* Lightweight dependency-free implementation

---

#  License

This project is created for educational and hackathon demonstration purposes.

---

#  Acknowledgements

Built as part of Hackathon 1.0 to demonstrate a zero-cost, fully browser-based employee performance management solution.

---

#  AtomQuest

A lightweight goal-setting and tracking portal built entirely with browser-native technologies.

