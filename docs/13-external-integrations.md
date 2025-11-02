## 13.1 Google Drive Principles

- Not every entity automatically generates folders — this is **configurable in Process Manager**
- Admin can define:

    - Which **processes** create folders
    - Which **file types** are accepted
    - Whether folder structure follows **DNA genealogy**
    - Whether permissions inherit from **Responsible / Controller**
 
- Example: PHO may generate a folder, but TSK or LOG may not

---

### 13.1.1 Implementation Example

```js
// Google Drive Integration for Document Storage
const driveIntegration = {
  authentication: "service_account",
  root_folder: "KOOL_TOOL_Calls",

  folder_structure: {
    pattern: "{year}/{month}/{process_type}/{dna_code}",
    example: "2025/09/PHO/PHO250001"
  },

  auto_upload: [
    {
      trigger: "call_completed",
      files: ["call_recording.mp3", "call_summary.pdf"]
    },
    {
      trigger: "contract_generated",
      files: ["contract_draft.docx"]
    }
  ],

  permissions: {
    default: "domain_readable",
    manager_override: "full_access"
  }
};
```

---

## 13.2 E-mail Notifications via Templates

```FileMaker
# FileMaker Send Mail Script

Set Variable [$to; Value: GetField("caller_email")]
Set Variable [$subject; Value: "Call Summary - " & PHO::DNA_Code]
Set Variable [$body; Value: Generate_Call_Summary()]
Set Variable [$attachment; Value: Generate_PDF_Report()]

Send Mail [
   Send via: SMTP Server;
   To: $to;
   Subject: $subject;
   Message: $body;
   Attachment: $attachment
]
```

---

## 13.3 SMS for Urgencies

```json
{
  "sms_gateway": {
    "provider": "Twilio",
    "api_endpoint": "https://api.twilio.com/2010-04-01",
    "from_number": "+40721KOOL",
    "templates": {
      "appointment_reminder": "Reminder: Call scheduled with {caller_name} at {time}",
      "follow_up": "Thank you for your call. Your request #{dna_code} is being processed."
    }
  }
}
```

---

## 13.4 CRM Integration (Salesforce)

```json
{
  "integration": "SALESFORCE",
  "configuration": {
    "endpoint": "https://kool-tool.my.salesforce.com/services/data/v52.0",
    "authentication": {
      "type": "OAuth2",
      "client_id": "${SF_CLIENT_ID}",
      "client_secret": "${SF_CLIENT_SECRET}",
      "refresh_token": "${SF_REFRESH_TOKEN}"
    },
    "mappings": {
      "PHO_to_SF": {
        "caller_name": "Contact.Name",
        "phone_number": "Contact.Phone",
        "outcome": "Task.Status",
        "notes": "Task.Description"
      },
      "SF_to_PHO": {
        "Contact.LastCallDate": "last_interaction",
        "Contact.AccountId": "company_id"
      }
    }
  },
  "sync": {
    "frequency": "realtime",
    "retry_policy": {
      "max_attempts": 3,
      "backoff": "exponential"
    },
    "error_handling": {
      "on_error": "queue_for_retry",
      "alert_after": 5
    }
  }
}
```
