# 5. Stack

## 5.1 Core Platform
At the heart of the system is **Claris FileMaker**, which serves as the:

- Primary application runtime and data storage engine.
- Orchestration layer for workflows, business logic, and server-side scripts.
- Hosting platform for both client access (Desktop, WebDirect, FM Go) and external integrations.

---

## 5.2 Front-End Layer
To support modern and dynamic interfaces, the system incorporates:

- **HTML/CSS/JavaScript** embedded in FileMaker Web Viewers for responsive and interactive UI components.
- Modular UI rendering based on JSON templates (e.g., forms, dashboards).
- Event-driven updates through FileMaker script callbacks or JavaScript bridges.

---

## 5.3 Data Access
For high-performance retrieval and flexible querying:

- **FileMaker ExecuteSQL** is used to fetch specific records, perform aggregates, and generate temporary datasets.
- SQL-based access simplifies filtering, sorting, and joining across tables (CMP, ETY, LOG).

---

## 5.4 Automation
The system supports robust automation through:

- **Server-Side FileMaker Scripts** – Scheduled events (e.g., nightly reports, trigger-based logic).
- **Script Triggers** – UI and data triggers (e.g., OnObjectSave, OnRecordCommit) to enforce validation and automations.
- Background queue execution for long-running processes (e.g., data syncs, notification batching).

---

## 5.5 Integration
The platform interacts with external systems through:

- **REST APIs** for integration with Google Workspace, Salesforce, Twilio, etc.
- **Webhooks** for push notifications and real-time data propagation.
- Reusable integration modules encapsulating authentication, error handling, and retries.
