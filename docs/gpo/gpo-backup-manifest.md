# GPO Backup Manifest

**Last Updated:** May 19, 2026  
**Domain:** lab.local

## Purpose
This manifest tracks all Group Policy Object backups, versions, and recovery information for the lab environment.

## Current Backups

| Backup Date | Backup ID (GUID) | GPO Name              | Status    |
|-------------|------------------|-----------------------|-----------|
| 2026-05-05  | {ABC-123...}     | Security-Baseline     | Verified  |
| 2026-05-05  | {DEF-456...}     | Engineering-Users     | Verified  |
| 2026-05-05  | {GHI-789...}     | SG-Engineering        | Verified  |

## Recovery Procedure

To restore a GPO from backup:

```powershell
Restore-GPO -BackupId "{GUID}" -Path "C:\GPOBackups" -Name "GPO-Name"
```

**Important:** Always verify security filtering and links after restoration.

**Related Files:**
- [GPO Overview](gpo-overview.md)
- [Security Baseline](gpo-security-baseline.md)
- [Engineering Department Baseline](engineering-gpo-baseline.md)
```