# Domain Admin Unable to RDP into WindowsADClient (SDESK-6)

**Last Updated:** April 28, 2026  
**Purpose:** Document the diagnosis and resolution of a Remote Desktop Services user rights issue that blocked Domain Admin RDP access after delegation testing.

## Lab Context

| Item              | Value                                      |
|-------------------|--------------------------------------------|
| Domain            | lab.local                                  |
| Affected Client   | WindowsADClient                            |
| Client OS         | Windows 10 Pro                             |
| Issue Type        | Remote Desktop Logon Rights / GPO          |
| Affected Account  | LAB\David (Domain Admin)                   |

## Problem Summary
After Help Desk delegation testing, the Domain Admin (`LAB\David`) could no longer connect via Remote Desktop to `WindowsADClient`. The error indicated missing rights to "log on through Remote Desktop Services."

## Resolution Steps

1. Confirmed Domain Controller access remained available.
2. Verified the issue was not password-related.
3. Checked local group memberships on the client.
4. Identified that the **"Allow log on through Remote Desktop Services"** user right was controlled by Group Policy.
5. Edited the relevant GPO on the Domain Controller and added `LAB\David`.
6. Forced Group Policy update on the client.
7. Verified successful RDP access was restored.

## Root Cause
The "Allow log on through Remote Desktop Services" user right was modified during delegation testing, removing Domain Admins / `LAB\David` from the allowed list.

## Validation
- Successful RDP connection as `LAB\David`.
- Full desktop access restored on `WindowsADClient`.

## Visual Evidence
**Full walkthrough available in:**  
`screenshots/sdesk-6-rdp-admin-access-denied/`

**Key Screenshots:**
- [01-rdp-access-denied-error.png](../../screenshots/sdesk-6-rdp-admin-access-denied/01-rdp-access-denied-error.png) — RDP login error
- [02-admin-rdp-lockout-issue.png](../../screenshots/sdesk-6-rdp-admin-access-denied/02-admin-rdp-lockout-issue.png) — Login prompt
- [03-allow-logon-through-remote-desktop-services.png](../../screenshots/sdesk-6-rdp-admin-access-denied/03-allow-logon-through-remote-desktop-services.png) — GPO policy
- [04-gpo-editor-remote-desktop-rights.png](../../screenshots/sdesk-6-rdp-admin-access-denied/04-gpo-editor-remote-desktop-rights.png) — GPO Editor view
- [05-remote-desktop-users-group.png](../../screenshots/sdesk-6-rdp-admin-access-denied/05-remote-desktop-users-group.png) — Remote Desktop Users group
- [06-regedit-auto-logon-disabled.png](../../screenshots/sdesk-6-rdp-admin-access-denied/06-regedit-auto-logon-disabled.png) — Registry check
- [07-rdp-access-restored-success.png](../../screenshots/sdesk-6-rdp-admin-access-denied/07-rdp-access-restored-success.png) — Access restored

## Lessons Learned
- User Rights Assignment in Group Policy overrides local settings and group membership.
- Delegation testing can unintentionally impact administrative RDP access.
- Always verify critical rights ("Allow log on through Remote Desktop Services") after GPO changes.
- Maintain emergency access methods (Azure Serial Console) when testing.

**Related Files:**
- [Tier 1 Help Desk Delegation Runbook](../runbooks/tier-1-support-delegation.md)
- [Helpdesk Delegation Runbook](../runbooks/helpdesk-delegation-runbook.md)

**Status:** Resolved
```
