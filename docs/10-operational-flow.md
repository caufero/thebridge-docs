## 10. Operational Flow

### 10.1 Process Manager Configuration Flow

#### 10.1.1 Description
This flow outlines how a manager defines and configures a process using the **Process Manager** interface.  
The configuration steps ensure that each process has its attributes, relationships, and workflow logic properly generated and tracked across the system.

---

#### 10.1.2 Flow Steps
1. **Manager selects or creates a process** in the TreeView interface.  
2. **Defines necessary attributes** required for the process.  
3. **Configures relationships and triggers** to link processes and automate behavior.  
4. **System automatically generates** the following components:
   - **Template in `CMP`** (Component Definition)
   - **User Interface** for data interaction
   - **Workflow in `ETY`** (Entity Definitions)
   - **Tracking in `LOG`** (Activity Logs)

---

#### 10.1.3 Notes
- The TreeView serves as the visual entry point for managing processes hierarchically.  
- Attributes define the data model, while relationships control how entities interact.  
- Triggers enable automation (e.g., automatic updates, notifications, or workflow transitions).  
- Generated elements maintain consistent linkage between CMP, ETY, and LOG tables for full traceability.  

---

#### 10.1.4 Example
When a new **“Client Onboarding”** process is created:
1. The manager defines attributes such as *Client Name*, *Assigned Officer*, and *Start Date*.  
2. A trigger is set to notify the assigned officer once onboarding begins.  
3. The system generates the necessary template (`CMP`), workflow (`ETY`), and logging rules (`LOG`) automatically.  
4. A user interface layout is also generated for process tracking and updates.  

---

#### 10.1.5 Process Manager Configuration Flow (in JSON)

##### 10.1.5.1 Description
This JSON structure defines the step-by-step actions performed by a manager during the configuration of a process within the **Process Manager** module.  
It includes the sequence of actions, required permissions, configurations for attributes, workflow setup, triggers, and validation before publication.

##### 10.1.5.2 JSON Definition

```json
{
  "manager_actions": [
    {
      "step": 1,
      "action": "Open Process Manager",
      "interface": "process_manager.fmp12",
      "permissions_required": ["process_admin"]
    },
    {
      "step": 2,
      "action": "Create New Process",
      "fields": {
        "process_code": "PHO",
        "process_name": "Phone Call Management",
        "standard_duration": 300
      }
    },
    {
      "step": 3,
      "action": "Define Attributes",
      "count": 5,
      "for_each": "Configure 10 domains"
    },
    {
      "step": 4,
      "action": "Configure Workflow",
      "states": 5,
      "transitions": 7
    },
    {
      "step": 5,
      "action": "Set Triggers",
      "rules": [
        "IF outcome = INTERESTED THEN create_offer",
        "IF duration > 600 THEN alert_manager"
      ]
    },
    {
      "step": 6,
      "action": "Validate and Publish",
      "validations": [
        "All required domains configured",
        "Workflow has end state",
        "At least one trigger defined"
      ]
    }
  ]
}
```

---

### 10.2 User Interaction Flow

#### 10.2.1 Description
This section defines the full interaction flow for a typical user within the system.  
It captures user login, dashboard configuration, and step-by-step actions during call creation.  
The flow is modeled as a JSON structure that illustrates how user inputs trigger system responses, validations, and integrations.

---

#### 10.2.2 JSON Definition

```js
// User Sara's Complete Interaction
const userFlow = {
  login: {
    user: "sara.vendite",
    role: "sales_operator",
    permissions: ["create_pho", "view_pho", "edit_own_pho"]
  },

  dashboard: {
    widgets: [
      "my_active_calls",
      "today_statistics",
      "k_coefficient_personal",
      "pending_follow_ups"
    ]
  },

  create_call: {
    steps: [
      {
        action: "click_new_call",
        system_response: "generate_form_from_template"
      },
      {
        action: "enter_caller_name",
        validation: "10_domains_checks",
        ml_assist: "auto_complete_suggestions"
      },
      {
        action: "save_call",
        system_actions: [
          "create_in_cmp",
          "init_workflow_in_ety",
          "log_in_log",
          "sync_to_crm",
          "calculate_k"
        ]
      }
    ]
  }
};
```

---

### 10.3 Business Scenario Flow

#### 10.3.1 Description
This section presents a real-world business scenario demonstrating how a sales operator interacts with the system using the **Phone Call Management (PHO)** process.  
It shows how a configured template, user actions, and triggers collaborate to produce measurable business outcomes (e.g., K Coefficient efficiency).

---

#### 10.3.2 Scenario Overview

| Field | Details |
|:------|:---------|
| **User** | Sara Bianchi *(Sales Department)* |
| **Customer** | Mario Rossi *(Boutique Milano)* |
| **Call Duration** | 12 minutes |
| **Outcome** | `INTERESTED` *(Triggers offer creation)* |
| **K Coefficient Result** | **1.49 ✅** *(Efficient)* |

---

#### 10.3.3 Process Summary
- The **PHO** process (Phone Call Management) was preconfigured and published in the system.  
- The user created a new call instance from the **PHO template** in `CMP`.  
- Upon saving the call with an **INTERESTED** outcome, the system automatically triggered an **offer creation** workflow.  
- The **K coefficient** (performance metric) was recalculated and updated in real-time.

---

#### 10.3.4 Initial Setup

##### 10.3.4.1 Description
Before any process instance can be created, a corresponding template must exist in the `CMP` table with the flag `is_template = true`.  
This template defines the structure, attributes, validation rules, workflow, and triggers that govern all subsequent PHO instances.

---

##### 10.3.4.2 Template JSON Definition

```json
{
  "dna_code": "PHO_TEMPLATE_V1",
  "is_template": true,
  "created_at": "2025-01-01T10:00:00Z",
  "created_by": "admin",
  "template_json": {
    "process_definition": {
      "code": "PHO",
      "name": "Phone Call Management",
      "category": "COMMUNICATION",
      "version": "1.0",
      "standard_duration_min": 10
    },
    "attributes": [
      {
        "name": "caller_name",
        "type": "TEXT",
        "required": true,
        "validation": "^[a-zA-Z\\s\\-]{2,255}$",
        "domains": {
          "identity": { "searchable": true, "unique": false },
          "temporal": { "versioning": true, "retention_days": 2555 },
          "authorization": { "read": ["sales", "admin"], "write": ["sales"] },
          "trigger": { "on_change": ["update_crm"] },
          "security": { "pii": true, "gdpr": true }
        }
      },
      {
        "name": "phone_number",
        "type": "PHONE",
        "required": true,
        "validation": "^\\+39\\s[0-9]{2}\\s[0-9]{7,8}$",
        "domains": {
          "identity": { "searchable": true },
          "security": { "encryption": "AES256" }
        }
      },
      {
        "name": "duration_minutes",
        "type": "NUMBER",
        "required": true,
        "validation": { "min": 0, "max": 999 },
        "domains": {
          "performance": { "affects_k": true }
        }
      },
      {
        "name": "outcome",
        "type": "SELECT",
        "required": true,
        "options": ["INTERESTED", "NOT_INTERESTED", "CALLBACK", "NO_ANSWER"],
        "domains": {
          "trigger": {
            "INTERESTED": ["create_offer", "notify_manager"],
            "CALLBACK": ["schedule_task"]
          }
        }
      },
      {
        "name": "notes",
        "type": "TEXTAREA",
        "required": false,
        "max_length": 1000,
        "domains": {
          "evolution": { "ml_training": true }
        }
      }
    ],
    "workflow": {
      "states": ["NEW", "IN_PROGRESS", "COMPLETED", "CANCELLED"],
      "transitions": [
        { "from": "NEW", "to": "IN_PROGRESS", "action": "start_call" },
        { "from": "IN_PROGRESS", "to": "COMPLETED", "action": "end_call" },
        { "from": "*", "to": "CANCELLED", "action": "cancel" }
      ]
    },
    "triggers": [
      {
        "name": "create_offer_on_interest",
        "condition": "outcome == 'INTERESTED'",
        "actions": [
          { "type": "create_process", "target": "OFC" },
          { "type": "notification", "recipient": "sales_manager" }
        ]
      }
    ]
  },
  "instance_json": null
}
```

#### 10.3.5 Creation of Instance (T = 0 seconds)

##### 10.3.5.1 Description
This section describes the real-time system actions that occur immediately when **Sara** initiates a new phone call process instance.  
At timestamp **10:00:00**, the system generates a new DNA code, creates the `CMP` record, initializes orchestration in `ETY`, and logs the creation in `LOG`.

---

##### 10.3.5.2 Generate DNA with Anti-Collision

###### 10.3.5.2.1 Description
When Sara clicks **[+ New Phone Call]**, the system must generate a unique DNA code for the instance.  
A locking mechanism ensures collision-free generation when multiple users create instances simultaneously.

###### 10.3.5.2.2 Code Implementation

```js
// DNA Generation with lock
function generateDNA() {
  const lock = acquireLock('PHO_DNA_GENERATION');
  if (!lock) {
    wait(100ms);
    return generateDNA(); // Retry
  }

  const year = '25';
  const lastDNA = query("SELECT MAX(dna_code) FROM CMP WHERE dna_code LIKE 'PHO25%'");
  const nextNum = extractNumber(lastDNA) + 1;
  const newDNA = 'PHO25' + String(nextNum).padStart(4, '0');

  releaseLock('PHO_DNA_GENERATION');
  return newDNA; // Returns: PHO250042
}
```

---

##### 10.3.5.3 Create CMP Instance Record

###### 10.3.5.3.1 Description
Once the DNA code has been generated, the system creates a new record in the **CMP (Component)** table.  
This entry is a live instance derived from the parent template `PHO_TEMPLATE_V1` and acts as the foundation for all subsequent operations across ETY (Entity) and LOG modules.

###### 10.3.5.3.2 JSON Definition

```json
{
  "dna_code": "PHO250042",
  "is_template": false,
  "parent_template": "PHO_TEMPLATE_V1",
  "created_at": "2025-01-15T10:00:00Z",
  "created_by": "sara.bianchi",
  "template_json": null,
  "instance_json": {
    "values": {
      "caller_name": null,
      "phone_number": null,
      "duration_minutes": null,
      "outcome": null,
      "notes": null
    },
    "metadata": {
      "ip_address": "192.168.1.45",
      "device": "Desktop_Chrome_v120",
      "location": "Office_Milano"
    }
  }
}
```

---

##### 10.3.5.4 Initialize ETY Orchestration

###### 10.3.5.4.1 Description
After the CMP instance record is created, the system initializes a corresponding **ETY (Entity)** orchestration entry.  
This defines the operational control layer for workflow management, responsibilities, and automation tracking tied to the instance.

###### 10.3.5.4.2 JSON Definition

```json
{
  "entity_id": "PHO250042",
  "process_type": "PHO",
  "workflow_state": "NEW",
  "responsible": "sara.bianchi",
  "controller": "sales_manager",
  "started_at": "2025-01-15T10:00:00Z",
  "completed_at": null,
  "workflow_json": {
    "current_state": "NEW",
    "available_actions": ["start_call", "cancel"],
    "state_timeout": null,
    "automation_enabled": true
  }
}
```

---

##### 10.3.5.5 First LOG Entry

###### 10.3.5.5.1 Description
Immediately after the creation of the CMP and ETY records, the system generates a **LOG entry** to document the event.  
This ensures complete traceability of all actions performed in the system.  
Each log entry is uniquely identified and includes contextual metadata for audit, debugging, and analytics.

###### 10.3.5.5.2 JSON Definition

```json
{
  "log_id": "550e8400-e29b-41d4-a716-446655440001",
  "entity_id": "PHO250042",
  "log_level": "L1_PROCESS",
  "action": "INSTANCE_CREATED",
  "actor": "sara.bianchi",
  "timestamp": "2025-01-15T10:00:00.123Z",
  "changes": {
    "type": "creation",
    "template": "PHO_TEMPLATE_V1",
    "initial_state": "NEW"
  },
  "metadata": {
    "session_id": "sess_abc123",
    "request_id": "req_xyz789"
  }
}
```
