## 16. Integration Strategy

### 16.1 Email Notifications  
- Actions defined in **Rules Engine** can trigger automatic email notifications  
- Target recipients may include:
  
      - Individual users  
      - Process **Responsible** or **Controller**  
      - Entire departments (e.g. Sales, Technical)

_Example:_  
`IF outcome = "INTERESTED" THEN email sales_manager@company.com`

---

### 16.2 Webhooks  
- Template actions can trigger **HTTP requests** to external systems  
- Enables real-time integration with:
  
      - CRMs  
      - Ticketing systems  
      - Analytics platforms  

_Example:_  
`IF task_created THEN POST to https://api.partner.com/events/task`

```json
{
  "webhook": {
    "method": "POST",
    "endpoint": "https://api.external-service.com/hooks/process",
    "payload": {
      "dna": "{{dna_code}}",
      "event": "PROCESS_COMPLETED",
      "timestamp": "{{timestamp}}"
    }
  }
}
```
