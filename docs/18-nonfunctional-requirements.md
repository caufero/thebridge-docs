# 18. Non-Functional Requirements

## 18.1 Performance  
- UI must remain **responsive for end users**
- Key operations (form load, save, search) complete within **< 500ms** under normal load
- Efficient JSON parsing and indexed fields for scalable data access

---

## 18.2 Security  
- **Role-based access control** at entity and field levels  
- **Field masking** for sensitive data (e.g. phone numbers, contract values)  
- **Full audit trail** for all sensitive or privileged actions  
- Encryption in transit (SSL/TLS) and at rest (AES-256 where supported)

---

## 18.3 Reliability  
- Application hosted on dedicated server (cloud or on-premise)  
- **Daily backups**, retained for minimum 30 days  
- **Weekly restore testing** to verify backup integrity  
- Triggers log retry and alert in case of delivery or workflow failure

---

## 18.4 Maintainability  
- Modular architecture based on **JSON process templates**  
- Minimal schema complexity (CMP, ETY, LOG as core tables)  
- Scripts are independent and properly documented  
- Rules defined declaratively within template JSON, not hardcoded

---

## 18.5 Availability  
- Target **business-hours uptime** (08:00–18:00 local time)  
- **Scheduled maintenance windows** during low-traffic periods  
- Fail-gracefully on non-critical services (e.g. CRM sync) with automatic retry

