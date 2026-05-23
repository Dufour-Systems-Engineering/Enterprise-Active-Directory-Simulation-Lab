# User Reports Access Denied to Department File Share (SDESK-3)

**Last Updated:** May 13, 2026  
**Purpose:** Document the resolution of a file share access issue caused by missing security group membership.

## Lab Context

| Item              | Value                                      |
|-------------------|--------------------------------------------|
| Domain            | lab.local                                  |
| Ticket            | SDESK-3                                    |
| Affected User     | test-user-Eng                              |
| Department        | Engineering                                |
| Share Server      | LAB-SHARE-01                               |
| Client            | WindowsADClient                            |
| Issue Type        | File Share Permissions / Group Membership  |

## Problem Summary
The Engineering user `test-user-Eng` received **Access Denied** when attempting to open the department file share (`LAB-SHARE-01`). Other Engineering users were unaffected.

## Resolution Steps

1. Reproduced the Access Denied error on `WindowsADClient`.
2. Confirmed basic connectivity and domain authentication were working.
3. Reviewed Share and NTFS permissions on the `LAB-SHARE-01` share.
4. Verified permissions were assigned via the Engineering share-access security group.
5. Checked the user’s group membership in Active Directory — user was missing from the required group.
6. Re-added `test-user-Eng` to the Engineering share-access group.
7. Refreshed the user session and retested access.

## Root Cause
The user was inadvertently removed from the required **Engineering share-access security group** during a recent access cleanup.

## Validation
- User confirmed successful access to the `LAB-SHARE-01` share.
- Expected folders and files were visible and accessible.
- No changes were needed to Share or NTFS permissions.

## Visual Evidence
**Full walkthrough available in:**  
`screenshots/sdesk-3-ad-share-access-denied/`

**Key Screenshots:**
- [01-access-denied-share.png](../../screenshots/sdesk-3-ad-share-access-denied/01-access-denied-share.png) — Access Denied error on share
- [02-user-group-membership-check.png](../../screenshots/sdesk-3-ad-share-access-denied/02-user-group-membership-check.png) — Missing group membership in ADUC
- [03-successful-share-access.png](../../screenshots/sdesk-3-ad-share-access-denied/03-successful-share-access.png) — Successful access after group membership restored

## Lessons Learned
- Always use security groups (AGDLP model) for share and NTFS permissions instead of direct user assignments.
- Group membership changes require a fresh logon session to take effect.
- Regularly audit group membership after bulk changes or access cleanups.

**Related Files:**
- [Finance User Workstation Authentication Failure (SDESK-2)](../troubleshooting/sdesk-2-finance-user-workstation-authentication-failure.md)
- [AD Password Reset - Finance Test User (SDESK-1)](../troubleshooting/sdesk-1-ad-password-reset-finance-user.md)
- [Group Management Runbook](../runbooks/group-management-runbook.md)

**Status:** Resolved