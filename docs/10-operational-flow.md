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

#### 10.1.5 Process Manager Configuration Flow (in JSON)

##### 10.1.5.1 Description
This JSON structure defines the step-by-step actions performed by a manager during the configuration of a process within the **Process Manager** module.  
It includes the sequence of actions, required permissions, configurations for attributes, workflow setup, triggers, and validation before publication.

---

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
