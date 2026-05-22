# Engineering Department GPO Baseline

**Last Updated:** May 19, 2026  
**Domain:** lab.local  
**Target OU:** OU=Engineering,OU=Accounts,DC=lab,DC=local

## Overview
This GPO set delivers department-specific user experience and computer configuration settings for the Engineering team while maintaining the domain security baseline.

## GPO Structure & Security Filtering

| GPO Name           | Type     | Target Security Group      | Permissions          | Status     |
|--------------------|----------|----------------------------|----------------------|------------|
| Engineering-Users  | User     | GG_Engineering_Users       | Read + Apply         | ✅ Applied |
| SG-Engineering     | Computer | SG-Engineering             | Read + Apply         | ✅ Applied |
| Domain Computers   | Computer | Domain Computers           | Read Only            | ✅ Applied |

**Note:** Authenticated Users was removed from security filtering to mitigate MS16-072 risks.

## User Configuration (Engineering-Users GPO)

| Setting                | Value / Configuration                                      | Status     |
|------------------------|------------------------------------------------------------|------------|
| Desktop Wallpaper      | `C:\CompanyShares\Eng-Dept-Wallpaper.jpg` (Fill style)     | ✅ Applied |
| Microsoft Edge Homepage| `https://google.com`                                       | ✅ Applied |
| Taskbar Layout         | XML-based pinned apps (Outlook, Excel, Word, VS Code, PowerShell, Edge, RDP) | ✅ Applied |
| Drive Mapping          | E: → `\\WindowsADClient.lab.local\CompanyShares\Engineering` | ✅ Applied |
| Folder Redirection     | Desktop & Documents → Engineering departmental share       | ✅ Applied |

## Note: When configuring wallpaper paths, always include the full file extension (e.g. `.jpg`). Omitting the extension can cause a black screen on client workstations.

## Computer Configuration (SG-Engineering GPO)

| Setting                     | Value                          | Status     |
|-----------------------------|--------------------------------|------------|
| PowerShell Execution Policy | RemoteSigned                   | ✅ Applied |
| Removable Media Access      | Read/Write allowed (unrestricted) | ✅ Applied |

## Design Notes
- Taskbar configuration uses Microsoft’s recommended XML Start Layout method for reliability.
- Folder redirection and drive mapping use direct UNC paths to the centralized `CompanyShares` share.
- Local Administrator rights for Engineering users were intentionally left intact in this lab environment.

## Related Files:
- [GPO Overview](gpo-overview.md)
- [Security Baseline](gpo-security-baseline.md)
- [GPO Backup Manifest](gpo-backup-manifest.md)