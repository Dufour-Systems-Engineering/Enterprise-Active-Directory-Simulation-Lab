# Runbooks

**Last Updated:** May 22, 2026  
**Domain:** lab.local  

This folder contains all the detailed, step-by-step runbooks for the Active Directory Domain Services lab. Each runbook documents real implementation work performed in the Azure Windows Server 2025 environment.

### Completed Runbooks (9)

| Runbook | Description | Status | Last Updated |
|---------|-------------|--------|--------------|
| [**OU Management Runbook**](./ou-management-runbook.md) | OU hierarchy design, creation (GUI + PowerShell), protection, renaming, cross-OU moves, and “Three Locks” troubleshooting | ✅ Complete | May 22, 2026 |
| [**Group Management Runbook**](./group-management-runbook.md) | AGDLP group strategy, GG_/DL_ creation, population, nesting, and management | ✅ Complete | May 22, 2026 |
| [**Backup & Restore Runbook**](./backup-&-restore-runbook.md) | System state backups, Azure VM restore points, Windows Server Backup feature, and pre-change safety net | ✅ Complete | May 22, 2026 |
| [**GPO Management Runbook**](./group-object-policy-runbook.md) | Pre-flight checks, manual GUI (Sales), automated PowerShell rollout (Engineering), wallpaper/drive mapping/folder redirection | ✅ Complete | May 22, 2026 |
| [**Bulk User Import Runbook**](./bulk-user-import.md) | Mockaroo schema + PowerShell import of ~1,000 realistic departmental users, data cleansing, error handling | ✅ Complete | May 21, 2026 |
| [**Network Share Setup Runbook**](./network-share-setup-runbook.md) | Company-wide file share creation, SMB configuration, Access-Based Enumeration decisions | ✅ Complete | May 22, 2026 |
| [**Helpdesk Delegation Runbook**](./helpdesk-delegation-runbook.md) | Tier 1 Help Desk delegation, password reset, unlock, and move testing | ✅ Complete | May 19, 2026 |
| [**Delegate User Moves Runbook**](./delegate-user-moves.md) | Cross-OU user object move permissions, before/after testing, final IT Admins scope | ✅ Complete | May 19, 2026 |
| [**Domain Join Runbook**](./domain-join-runbook.md) | Client workstation domain join (WADC2), Azure VNet DNS, RDP break/fix troubleshooting | ✅ Complete | May 20, 2026 |

**Total Files in this folder:** 9  
**Fully Complete:** 9

**Related Links**  
- [Root README](../../README.md)  
- [CHANGELOG](../../CHANGELOG.md)  
- [Architecture](../architecture/)  
- [GPO](../gpo/)  
- [Delegation](../delegation/)