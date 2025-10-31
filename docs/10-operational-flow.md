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
