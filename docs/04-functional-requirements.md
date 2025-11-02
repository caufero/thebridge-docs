# 4. Functional Requirements

## 4.1 Process Template Authoring
The system must allow administrators to:

- Create, edit, and publish **process templates** through a graphical interface.
- Define **attributes** (fields), **workflow steps**, **role assignments**, and **business rules** for each template.
- Version templates, allowing older process instances to retain their original structure.
- Validate template structure before publishing (required fields, workflow completeness, etc.).

---

## 4.2 Instance Lifecycle
The platform must support the full lifecycle of a process instance:

- **Start:** Users can initiate new process instances from the available templates.
- **View & Edit:** Users can view and update allowed fields based on role and state.
- **Advance:** Workflow transitions must be enforced and logged, including automated or manual state advancements.
- **Complete or Cancel:** Finalize the process or cancel it with proper audit logs.

---

## 4.3 Entity Management
Entities (people, companies, products, etc.) must be treated as **first-class processes**:

- Every entity is instantiated from a template (e.g., **STAFF**, **CLIENT**, **PRODUCT**).
- Entity data is editable and versioned through controlled workflows.
- Related processes (e.g., PHO → OFC → ORD) inherit entity data through configuration.

---

## 4.4 Approvals & Denials

- Each approval step must capture the reviewer, timestamp, and optional notes.
- Denials must include a reason and trigger follow-up actions or rollbacks.
- System-wide audit must reflect the change history for approval decisions.

---

## 4.5 Reporting & Dashboards

- The system must generate **daily operational reports** driven by the central CMP-ETY-LOG ledger.
- Provide **dashboards** for workload, performance tracking, and KPIs (e.g., K coefficient).
- Export reports to CSV, XLSX, or PDF for external analysis.

---

## 4.6 Notifications

- Trigger-based **email notifications** must be supported using configurable templates.
- Possible notification targets: users, managers, controllers, distribution lists.
- Notification events configurable at **template level**.

---

## 4.7 Authentication & Authorization

- Standard **FileMaker authentication** (accounts, privilege sets).
- **Role-based visibility**: users only see what their role and process state allow.
- Support **field-level access control** (e.g., hidden, read-only, masked).
- **Validation rules** enforced both client-side and server-side for security.
