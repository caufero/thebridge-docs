# 21. Deployment Plan

## 21.1 Server Setup  
Deployment is performed directly on Luca’s existing FileMaker Server instance.

- **Server**: FileMaker Server (macOS or Windows)
- **Deployment Method**: Direct upload or remote transfer of `.fmp12` file
- **App File**: `TheBridge.fmp12` (single consolidated file including all modules)
- **Environment**: Production instance with SSL enabled  
- **Client Access**: FileMaker Pro / WebDirect (optional, based on license)

---

## 21.2 Backups  
Automatic daily backups with retention policies to support disaster recovery.

- **Frequency**: Nightly (1 AM server time)
- **Retention**: 14 to 30 days  
- **Storage**: Local drive + optional synced cloud (Google Drive or OneDrive)  
- **Disaster Recovery**: Weekly backup integrity tests with restore simulation  
- **Backup Scope**: Includes file, server config, and external container data (if used)
