## 17. Reporting & Analytics

### 17.1 Operational Reports  
- Built dynamically from core tables:
   
    - **Processes** (business events)  
    - **ETY** (entity values and states)  
    - **Logs** (actions and timeline)
 
- Default UI includes:
   
    - List views  
    - KPI dashboards  
    - Timeline-based activity views  

---

### 17.2 Saved Reports  
- Reports are **not stored statically**  
- They are **generated on demand**, especially when:
   
    - A process completes  
    - A trigger fires (`on_success`, `on_timeout`)  
    - A user explicitly requests via UI

_Example:_  
`Generate “Weekly Follow-Up Summary” when last call is logged on Friday`

---

### 17.3 Export Options  
- Users can export results in multiple formats:
  
    - **CSV** (data interchange)  
    - **XLSX** (analysis in Excel / Sheets)  
    - **PDF** (print-ready reports)

```json
{
  "export": {
    "formats": ["csv", "xlsx", "pdf"],
    "default": "xlsx",
    "filename_pattern": "{process_type}_{date}_report"
  }
}
```
