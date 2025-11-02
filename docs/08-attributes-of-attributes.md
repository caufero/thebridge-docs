Each attribute is not just a data field but a complete entity managing **10 existential domains**.

---

## 8.1 The 10 Domains for Each Attribute

| **Domain** | **Behavior** | **PHO Implementation** |
|-------------|---------------|--------------------------|
| **IDENTITY** | Who I am | Unique identifier in call context |
| **TEMPORAL** | When I exist | Versioning of call data |
| **AUTHORIZATION** | Who controls me | Role-based access |
| **COMMUNICATION** | How I speak | API/sync protocols |
| **TRIGGER** | What I activate | Automated actions |
| **DOCUMENT** | What I link | Related documents |
| **MATERIAL** | My limits | Data constraints |
| **PERFORMANCE** | How I measure | KPIs and metrics |
| **SECURITY** | How I protect | Encryption / GDPR |
| **EVOLUTION** | How I improve | ML optimization |

---

## 8.2 Example of the Attributes of Attributes Under the 10 Domains

#### 1. IDENTITY
```json
{
  "code": "CUST_NAME",
  "type": "text",
  "unique_context": true,
  "genealogy": "parent_attribute"
}
```

#### 2. TEMPORAL
```json
{
  "valid_from": "creation_date",
  "valid_until": "contract_end",
  "historical": true,
  "update_frequency": "on_change"
}
```

#### 3. AUTHORIZATION
```json
{
  "view": ["all_users"],
  "edit": ["sales", "admin"],
  "delete": ["admin_only"],
  "privacy_mask": true
}
```

#### 4. COMMUNICATION
```json
{
  "api_field": "customer_name",
  "export_formats": ["JSON", "CSV"],
  "sync_external": ["CRM_system"],
  "validation_api": "check_duplicate"
}
```

#### 5. TRIGGERS
```json
{
  "on_change": "validate_customer",
  "on_empty": "block_save",
  "on_duplicate": "merge_suggestion",
  "cascade_update": true
}
```

#### 6. DOCUMENTARY
```json
{
  "label": "Customer Name",
  "help_text": "Enter full name",
  "format_output": "uppercase",
  "include_reports": true
}
```

#### 7. MATERIAL
```json
{
  "max_length": 100,
  "storage_bytes": 200,
  "encryption": true,
  "indexed": true
}
```

#### 8. PERFORMANCE
```json
{
  "input_time": "5_seconds",
  "quality_weight": 0.3,
  "affects_kpi": "data_quality",
  "benchmark": "industry_standard"
}
```

#### 9. SECURITY
```json
{
  "gdpr_relevant": true,
  "audit_all_changes": true,
  "encryption": "AES256",
  "retention": "7_years"
}
```

#### 10. EVOLUTION
```json
{
  "ml_training": true,
  "pattern_detection": true,
  "auto_complete": true,
  "learn_from_usage": true
}
```

Each attribute is a living construct — it defines how data behaves, evolves, and interacts within the ontological model. Together, these ten domains create a **self-regulating and auditable framework** that governs the lifecycle, control, and intelligence of every attribute in the system.

---

## 8.3 Universal Meta-Properties (SUPER TABLE)

| Property     | Values        | Description |
|---------------|---------------|--------------|
| **Filterable** | True / False | Usable in filters |
| **Sortable**   | True / False | Can sort results |
| **Groupable**  | True / False | Can group data |
| **Exportable** | True / False | Include in exports |

These properties enable the **Super Table** concept — the system behaves like a **universal Excel sheet** where every element, process, and attribute becomes globally filterable, sortable, and exportable. This design ensures consistency across modules and provides an instant analytical layer over all data structures.

---

## 8.4 Example of Meta-Attributes for `Caller_Name` Attribute of Phone Call Template

```json
"1_IDENTITY": {
  "code": "PHO_ATTR_001",
  "display_name": "Caller Name",
  "description": "Person or company identification",
  "searchable": true,
  "unique_in_context": false,
  "dna_component": "PHO.COMM.001"
},

"2_TEMPORAL": {
  "versioning": true,
  "historical_depth": "FULL",
  "retention_days": 2555,
  "track_changes": {
    "what": true,
    "who": true,
    "when": true,
    "why": true
  },
  "temporal_validity": {
    "valid_from": "call_start",
    "valid_until": "archived"
  }
},

"3_AUTHORIZATION": {
  "permissions": {
    "sales": {"read": true, "write": true, "delete": false},
    "production": {"read": true, "write": false, "delete": false},
    "finance": {"read": false, "write": false, "delete": false},
    "admin": {"read": true, "write": true, "delete": true}
  },
  "special_rules": {
    "mask_in_reports": false,
    "require_approval_for_change": false,
    "audit_all_access": true
  }
},

"4_COMMUNICATION": {
  "api_mapping": {
    "rest": "customer_name",
    "graphql": "customerIdentity.fullName",
    "soap": "CUST_NAME"
  },
  "export_formats": {
    "csv": "Cliente",
    "json": "customer_name",
    "xml": "<CustomerName>"
  },
  "sync_systems": [
    {
      "system": "CRM_SALESFORCE",
      "field": "Contact.Name",
      "bidirectional": true,
      "sync_frequency": "realtime"
    },
    {
      "system": "ERP_SAP",
      "field": "KUNNR_NAME",
      "bidirectional": false,
      "sync_frequency": "batch_hourly"
    }
  ]
},

"5_TRIGGER": {
  "on_create": [
    {
      "condition": "value != null",
      "action": "search_customer_history",
      "async": true
    }
  ],
  "on_change": [
    {
      "condition": "old_value != new_value",
      "action": "update_all_related_processes",
      "scope": "all_open_processes"
    },
    {
      "condition": "contains('VIP')",
      "action": "alert_management",
      "priority": "high",
      "notification": "immediate"
    }
  ],
  "on_validate": [
    {
      "condition": "length < 2",
      "action": "validation_error",
      "message": "Name too short"
    }
  ]
},

"6_DOCUMENT": {
  "auto_attach": {
    "contracts": {
      "match_by": "customer_name",
      "last_n": 3,
      "file_types": ["pdf", "docx"]
    },
    "correspondence": {
      "match_by": "email_domain",
      "date_range": "last_6_months"
    }
  },
  "generate_on_complete": {
    "call_summary": {
      "template": "call_summary_template.docx",
      "format": "pdf",
      "storage": "gdrive://calls/{date}/{caller_name}"
    }
  },
  "link_to_existing": {
    "search_in": ["contracts", "quotes", "invoices"],
    "match_algorithm": "fuzzy",
    "confidence_threshold": 0.85
  }
},

"7_MATERIAL": {
  "data_type": "string",
  "constraints": {
    "min_length": 2,
    "max_length": 100,
    "pattern": "^[A-Za-zÀ-ÿ\\s\\-\\.]+$",
    "encoding": "UTF-8"
  },
  "storage": {
    "bytes_allocated": 200,
    "compression": false,
    "encryption": true
  },
  "performance": {
    "indexed": true,
    "cached": true,
    "lazy_load": false
  }
},

"8_PERFORMANCE": {
  "metrics": {
    "input_time": {
      "target_seconds": 5,
      "warning_threshold": 10,
      "measure": true
    },
    "accuracy": {
      "error_rate_target": 0.02,
      "validation_rules": ["not_empty", "proper_format"]
    },
    "corrections_frequency": {
      "track": true,
      "alert_threshold": 3
    }
  },
  "k_contribution": {
    "weight": 0.15,
    "factors": {
      "corrections": "negative",
      "speed": "positive",
      "completeness": "positive"
    }
  }
},

"9_SECURITY": {
  "classification": "PII",
  "encryption": {
    "at_rest": "AES-256",
    "in_transit": "TLS-1.3",
    "key_rotation": "90_days"
  },
  "compliance": {
    "GDPR": {
      "is_personal_data": true,
      "lawful_basis": "legitimate_interest",
      "retention_period": "7_years",
      "right_to_deletion": true,
      "portability": true
    }
  },
  "audit": {
    "log_all_access": true,
    "log_all_changes": true,
    "suspicious_patterns": ["bulk_export", "after_hours_access"],
    "alert_security_team": true
  }
},

"10_EVOLUTION": {
  "machine_learning": {
    "auto_complete": {
      "enabled": true,
      "algorithm": "frecency",
      "min_chars": 3,
      "max_suggestions": 5,
      "learn_from_corrections": true
    },
    "pattern_detection": {
      "identify_duplicates": true,
      "suggest_merge": true,
      "confidence_threshold": 0.85
    },
    "anomaly_detection": {
      "unusual_values": true,
      "statistical_outliers": true,
      "alert_on_anomaly": true
    }
  },
  "continuous_improvement": {
    "collect_feedback": true,
    "track_corrections": true,
    "optimize_suggestions": true,
    "update_frequency": "weekly"
  }
}
```

Each meta-attribute operates as a **domain of intelligence** defining not only the behavior and structure of the `Caller_Name` field but also its governance, learning, and compliance lifecycle within the Phone Call ontology.

---

## 8.5 Example of Meta-Attributes for `Caller_Name` Attribute of Phone Call Template

```json
"1_IDENTITY": {
  "code": "PHO_ATTR_001",
  "display_name": "Caller Name",
  "description": "Person or company identification",
  "searchable": true,
  "unique_in_context": false,
  "dna_component": "PHO.COMM.001"
}
```

---

## 8.6 FileMaker Script – Process the 10 Domains

### 8.6.1 Description
This script iterates through all ten domains associated with a given attribute and delegates the processing of each domain to its corresponding sub-script.  
It ensures that every domain type (Identity, Temporal, etc.) is handled in sequence according to its position in the JSON structure.

---

### 8.6.2 Script Name
`Process_10_Domains`

---

### 8.6.3 Script Logic

```plaintext
# Process all 10 domains for an attribute
Set Variable [$attribute_name; Value: Get(ScriptParameter)]
Set Variable [$domains_json; Value: JSONGetElement(CMP::Template_JSON; 
   "attributes." & $attribute_name & ".domains")]

# Loop through each domain
Set Variable [$i; Value: 1]
Loop
   Exit Loop If [$i > 10]

   Set Variable [$domain; Value: JSONGetElement($domains_json; $i)]

   # Process based on domain type
   If [$i = 1]  # IDENTITY
      Perform Script ["Process_Identity_Domain"; Parameter: $domain]
   Else If [$i = 2]  # TEMPORAL
      Perform Script ["Process_Temporal_Domain"; Parameter: $domain]
   # ... continue for all 10 domains
   End If

   Set Variable [$i; Value: $i + 1]
End Loop
```

---

## 8.7 FileMaker Function – Parse Attribute Domains as JSON

### 8.7.1 Description
This custom FileMaker function parses all domains defined within an attribute’s JSON structure.  
It iterates through each domain key inside the `"domains"` object and processes each one individually using the `Process_Single_Domain` function.  
The function returns the combined output of all processed domains or an error message if no domains are found.

---

### 8.7.2 Function Name
`Parse_Attribute_Domains_As_JSON`

---

### 8.7.3 Function Logic

```plaintext
Let ([
   domains = JSONListKeys(json_text; "domains");
   result = ""
];
Case(
   not IsEmpty(domains);
   While([
      i = 1;
      output = ""
   ];
   i ≤ ValueCount(domains);
   [
      domain = GetValue(domains; i);
      config = JSONGetElement(json_text; "domains." & domain);
      output = output & Process_Single_Domain(domain; config) & ¶;
      i = i + 1
   ];
   output
   );
   "Error: No domains found"
)
)
```
