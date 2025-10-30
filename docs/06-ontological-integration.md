# 6. Ontological Integration

## 6.1 Instance DNA Format

### 6.1.1 Format
`PRXYYNNNN`

#### 6.1.1.1 Where
- `PRX` → 3-letter **process code** (e.g., `TSK`, `RCH`, `TEH`, `PHO`, `OFC`, `BOM`, etc.)
- `YY` → **Year** (e.g., `25` for 2025)
- `NNNN` → **Sequential number** with anti-collision logic for multi-user environments.  
  Starts at 4 digits and automatically expands as needed.

#### 6.1.1.2 Examples
- `PHO250001` → First phone call in 2025  
- `PHO259999` → 9999th phone call  
- `PHO2510000` → 10,000th phone call (auto-expands to 5 digits)  
- `PHO25100000` → 100,000th phone call (auto-expands to 6 digits)

---

### 6.1.2 FileMaker Implementation

#### 6.1.2.1 Simple
```filemaker
DNA_Generator = "PHO" & Right ( Year ; 2 ) & SerialIncrement ( PHO_Counter ; 4 )

// SerialIncrement automatically manages digit overflow
```

#### 6.1.2.2 Extended
```filemaker
# Generate unique DNA code for PHO
Set Variable [$year; Value: Right(Year(Get(CurrentDate)); 2)]
Set Variable [$type; Value: "PHO"]

# Get next sequence with collision prevention
Perform Script ["Get_Next_Sequence"; Parameter: "PHO"]
Set Variable [$sequence; Value: Get(ScriptResult)]

# Format with automatic overflow handling
If [$sequence < 10000]
    Set Variable [$dna; Value: $type & $year & Right("0000" & $sequence; 4)]
Else If [$sequence < 100000]
    Set Variable [$dna; Value: $type & $year & $sequence]
End If

Exit Script [Result: $dna]
```

## 6.2 Correct Nomenclature for Architecture (CRITICAL)

| **Layer**     | **Code** | **Description**               |
|----------------|----------|-------------------------------|
| **Processes**  | CMP      | Components / Catalog          |
| **Entities**   | ETY      | Entity / Orchestrator         |
| **Logs**       | LOG      | Actions / Registry            |

Each layer represents a core dimension of *The Bridge* ontology:

- **CMP (Processes)** define reusable templates and behaviors.  
- **ETY (Entities)** orchestrate process executions and hold contextual data.  
- **LOG (Logs)** capture every action, transition, and state change.

Together, they ensure that *every operation in the system is observable, traceable, and reproducible* — forming the backbone of the 3P3 architectural model.

## 6.3 CMP-ETY-LOG Architecture Clarification

### 6.3.1 CMP (Components)

The **CMP** table is the foundation of the system. It contains *everything that can exist* — both **process templates** and **real instances** that hold business data.

#### 6.3.1.1 Contents of CMP
- Process templates (e.g., `PROC_PHO`, `PROC_RCH`)
- Real instances with business data and attributes (e.g., `PHO250001` with customer name, duration, etc.)
- Attributes
- Meta-attributes

#### 6.3.1.2 CMP Table Definition and Example
The `CMP` table stores both templates and instances in a unified, flexible structure.

```sql
CREATE TABLE CMP (
    ID UUID PRIMARY KEY,
    DNA_Code TEXT UNIQUE,
    Template_ID TEXT,
    Template_JSON JSON,
    Instance_JSON JSON,
    Attributes_Metadata_JSON JSON,
    Created TIMESTAMP,
    Creator TEXT,
    Modified TIMESTAMP,
    Modifier TEXT
);
```

Example of Instance_JSON in CMP (Phone Call: PHO250001)
```json
{
    "caller": "Mario",
    "duration": 18,
    "outcome": "Sale"
}
```

### 6.3.2 ETY (Entities)

The **ETY** table represents everything that exists *right now* — the live orchestration layer of the system. It is responsible for managing **workflow states**, **transitions**, and **responsible parties**, but it does **not** hold business data.

#### 6.3.2.1 Contents of ETY
- Everything that exists now  
- Workflow orchestration **only**  
- States, transitions, and responsible parties  
- No business data (business data lives in CMP)

#### 6.3.2.2 ETY Table Definition and Example
The `ETY` table manages the dynamic behavior of active processes.

```sql
CREATE TABLE ETY (
    ID UUID PRIMARY KEY,
    Entity_ID TEXT UNIQUE,
    Process_Type TEXT,
    Workflow_State TEXT,
    Workflow_JSON JSON,
    Responsible TEXT,
    Controller TEXT,
    Started_at TIMESTAMP,
    Completed_at TIMESTAMP
);
```

Example of data in ETY for same Phone Call (PHO250001_ORCH)
```json
{
    "status": "In Progress",
    "responsible": "Sara"
}
```

### 6.3.3 LOG (Logs)

The **LOG** table is the immutable history of everything that has happened in the system.  
It serves as a **complete audit trail** — recording every action, change, and event that occurs during a process lifecycle.

#### 6.3.3.1 Contents of LOG
- Everything that has happened  
- Immutable history of every action  
- Complete audit trail of the system  

#### 6.3.3.2 LOG Table Definition and Example
The `LOG` table captures and preserves all events and transitions across processes and entities.

```sql
CREATE TABLE LOG (
    ID UUID PRIMARY KEY,
    Log_ID UUID,
    Entity_ID TEXT,
    Log_Level TEXT,
    Action TEXT,
    Actor TEXT,
    Changes_JSON JSON,
    Metadata_JSON JSON,
    Timestamp TIMESTAMP
);
```

Example of data in LOG for same Phone Call (PHO250001)
```json
{
    "created": "Updated duration",
    "completed": "Triggered OFC"
}
```

#### 6.3.3.3 Log Granularity (Three-Level Hierarchy)

The logging system operates on a **three-level hierarchy**, allowing The Bridge to record activities at varying levels of depth and frequency. This ensures both high-level summaries and precise traceability for every process event.

##### 6.3.3.3.1 Levels of Granularity

```json
"L1_PROCESS": {
  "description": "Major lifecycle events",
  "frequency": "low",
  "examples": ["process_created", "process_completed", "process_archived"],
  "data_captured": {
    "process_id": "PHO250001",
    "template_version": "V1",
    "total_duration": 240,
    "k_coefficient": 1.35
  }
},

"L2_ACTIVITY": {
  "description": "Workflow transitions and significant actions",
  "frequency": "medium",
  "examples": ["status_change", "assignment_change", "approval_given"],
  "data_captured": {
    "from_state": "NEW",
    "to_state": "IN_PROGRESS",
    "transition_reason": "call_started",
    "responsible_user": "USR_SARA"
  }
},

"L3_ATOMIC": {
  "description": "Every attribute modification",
  "frequency": "high",
  "examples": ["field_updated", "validation_failed", "auto_correction"],
  "data_captured": {
    "attribute": "caller_name",
    "old_value": "Rossi",
    "new_value": "Mario Rossi",
    "change_reason": "user_enrichment",
    "timestamp_precise": "2025-09-17T09:16:23.456Z"
  }
}
```

#### 6.3.3.4 LOG to RAP (Report) Transformation

Automatic **Daily Activity Report** generation from the immutable **LOG** ledger.  
Transformation rules define filters, aggregations, formats, and delivery channels.

```json
{
  "rap_configuration": {
    "source": "LOG",
    "transformation_rules": {
      "daily_user_activity": {
        "filter": "WHERE actor = :user AND date = :today",
        "aggregate": [
          "COUNT(*) as total_actions",
          "COUNT(DISTINCT entity_id) as processes_touched",
          "AVG(duration) as avg_process_time",
          "SUM(CASE WHEN outcome = 'INTERESTED' THEN 1 ELSE 0 END) as conversions"
          ],
        "output_format": "PDF",
        "delivery": "email_at_18:00"
      },
      "process_efficiency": {
        "group_by": "process_type",
        "metrics": ["k_coefficient", "cycle_time", "error_rate"],
        "visualization": "dashboard_widget"
      }
    }
  }
}
```

#### 6.3.3.5 LOG to RAP (Report) Transformation — FileMaker Script to Generate RAP Report

```filemaker
// Generate Daily Activity Report from LOG
Set Variable [ $user ; Value: Get ( AccountName ) ]
Set Variable [ $date ; Value: Get ( CurrentDate ) ]

// Query LOG table (conceptual; replace with your SQL/CF/plugin approach)
Set Variable [ $sql ; Value: "
   SELECT
      COUNT(*) AS total_actions,
      COUNT(DISTINCT entity_id) AS processes,
      AVG(CASE WHEN action = 'call_completed'
         THEN JSON_EXTRACT(metadata, '$.duration') END) AS avg_duration
   FROM LOG
   WHERE actor = ? AND DATE(timestamp) = ?
" ]

// Execute query (via plugin/CF) and capture results
Perform SQL [ Query: $sql ; Parameters: $user & ¶ & $date ]
Set Variable [ $results ; Value: Get ( ScriptResult ) ]

// Render report (HTML) using a template
Set Variable [ $report_html ; Value: Process_RAP_Template ( $results ) ]

// Produce PDF and deliver (email/schedule handled by wrapper script)
Perform Script [ "Generate_PDF" ; Parameter: $report_html ]
```
