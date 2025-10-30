# 6. Ontological Integration

## 6.1 Instance DNA Format

### Format
`PRXYYNNNN`

#### Where
- `PRX` → 3-letter **process code** (e.g., `TSK`, `RCH`, `TEH`, `PHO`, `OFC`, `BOM`, etc.)
- `YY` → **Year** (e.g., `25` for 2025)
- `NNNN` → **Sequential number** with anti-collision logic for multi-user environments.  
  Starts at 4 digits and automatically expands as needed.

#### Examples
- `PHO250001` → First phone call in 2025  
- `PHO259999` → 9999th phone call  
- `PHO2510000` → 10,000th phone call (auto-expands to 5 digits)  
- `PHO25100000` → 100,000th phone call (auto-expands to 6 digits)

---

### FileMaker Implementation

#### Simple
```filemaker
DNA_Generator = "PHO" & Right ( Year ; 2 ) & SerialIncrement ( PHO_Counter ; 4 )

// SerialIncrement automatically manages digit overflow
```

#### Extended
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

### CMP (Components)

The **CMP** table is the foundation of the system. It contains *everything that can exist* — both **process templates** and **real instances** that hold business data.

#### Contents of CMP
- Process templates (e.g., `PROC_PHO`, `PROC_RCH`)
- Real instances with business data and attributes (e.g., `PHO250001` with customer name, duration, etc.)
- Attributes
- Meta-attributes

#### CMP Table Definition and Example
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

### ETY (Entities)

The **ETY** table represents everything that exists *right now* — the live orchestration layer of the system. It is responsible for managing **workflow states**, **transitions**, and **responsible parties**, but it does **not** hold business data.

#### Contents of ETY
- Everything that exists now  
- Workflow orchestration **only**  
- States, transitions, and responsible parties  
- No business data (business data lives in CMP)

#### ETY Table Definition and Example
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

### LOG (Logs)

The **LOG** table is the immutable history of everything that has happened in the system.  
It serves as a **complete audit trail** — recording every action, change, and event that occurs during a process lifecycle.

#### Contents of LOG
- Everything that has happened  
- Immutable history of every action  
- Complete audit trail of the system  

#### LOG Table Definition and Example
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
