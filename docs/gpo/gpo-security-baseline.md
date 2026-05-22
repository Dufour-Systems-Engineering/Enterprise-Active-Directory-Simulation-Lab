# Security Baseline GPO

**Last Updated:** May 19, 2026  
**Domain:** lab.local  
**Scope:** Domain-wide

## Purpose
This GPO establishes the core security hardening settings applied across all computers in the lab environment, based on Microsoft Security Baselines and CIS Level 1 recommendations.

## Password & Account Policies

| Setting                    | Value              | Status     |
|----------------------------|--------------------|------------|
| Enforce password history   | 24 passwords       | ✅ Applied |
| Maximum password age       | 42 days            | ✅ Applied |
| Minimum password length    | 14 characters      | ✅ Applied |
| Account lockout threshold  | 10 attempts        | ✅ Applied |
| Account lockout duration   | 15 minutes         | ✅ Applied |

## Session & Display Security

| Setting                    | Value                  | Status     |
|----------------------------|------------------------|------------|
| Screen saver timeout       | 10 minutes             | ✅ Applied |
| Password-protected screen saver | Enabled           | ✅ Applied |

## Advanced Security Settings

- **SMBv1** — Disabled via GPO to eliminate legacy vulnerabilities
- **LDAP Signing** — Required for all domain controller communications
- **Hardened UNC Paths** — Configured for `\\*\SYSVOL` and `\\*\NETLOGON`
- **Audit Policies**:
  - Logon / Logoff → Success & Failure
  - Account Management → Success & Failure
  - Object Access → Success only

## PowerShell & Scripting

- **Execution Policy**: `RemoteSigned`
- **Script Execution**: Enabled

## Removable Media
- Read/Write access allowed (unrestricted) for operational flexibility in the lab environment

**Related Files:**
- [GPO Overview](gpo-overview.md)
- [Engineering Department Baseline](engineering-gpo-baseline.md)
- [GPO Backup Manifest](gpo-backup-manifest.md)