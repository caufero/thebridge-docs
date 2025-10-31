## 9. Workflow Specification

### 9.1 Description
This section defines the workflow configuration for process-driven entities such as calls, tickets, or tasks.  
It outlines the **states**, **transitions**, and **automations** that determine how an entity moves through its lifecycle.  
The specification follows a JSON-based structure, which can be parsed and executed dynamically by the system.

---

### 9.2 JSON Definition

```json
{
  "workflow": {
    "states": [
      {
        "code": "NEW",
        "name": "New Call",
        "allowed_transitions": ["IN_PROGRESS", "CANCELLED"],
        "timeout": null,
        "auto_assign": true
      },
      {
        "code": "IN_PROGRESS",
        "name": "Call Active",
        "allowed_transitions": ["COMPLETED", "FOLLOW_UP", "ESCALATED"],
        "timeout": 3600,
        "timer_running": true
      },
      {
        "code": "COMPLETED",
        "name": "Call Completed",
        "allowed_transitions": ["ARCHIVED", "REOPENED"],
        "triggers": ["generate_summary", "update_statistics"]
      },
      {
        "code": "FOLLOW_UP",
        "name": "Follow-up Required",
        "allowed_transitions": ["IN_PROGRESS", "COMPLETED"],
        "creates": "TSK_CALLBACK"
      },
      {
        "code": "ARCHIVED",
        "name": "Archived",
        "allowed_transitions": [],
        "final": true,
        "retention": "7_years"
      }
    ],
    "transitions": [
      {
        "from": "NEW",
        "to": "IN_PROGRESS",
        "action": "start_call",
        "permission": "sales",
        "log_level": "L2_ACTIVITY"
      },
      {
        "from": "IN_PROGRESS",
        "to": "COMPLETED",
        "action": "end_call",
        "required_fields": ["outcome", "duration"],
        "log_level": "L1_PROCESS"
      }
    ]
  }
}
```
