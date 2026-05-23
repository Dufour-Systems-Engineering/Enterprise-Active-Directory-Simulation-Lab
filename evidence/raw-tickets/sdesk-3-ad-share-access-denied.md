# AD DS Break/Fix: User Reports Access Denied to Department File Share (SDESK-3)

## Lab Context

| Item              | Value                                      |
|-------------------|--------------------------------------------|
| Domain            | lab.local                                  |
| Ticket            | SDESK-3                                    |
| Affected User     | test-user-Eng                              |
| Department        | Engineering                                |
| Share Server      | LAB-SHARE-01                               |
| Client            | WindowsADClient                            |
| Issue Type        | File share permissions / Group membership  |

## Problem Summary
Engineering user `test-user-Eng` received **Access Denied** when trying to open the department file share after a recent access cleanup. Other Engineering users were unaffected.

## Troubleshooting Steps
1. Reproduced Access Denied on the client.
2. Confirmed connectivity and basic domain authentication worked.
3. Reviewed Share and NTFS permissions on LAB-SHARE-01.
4. Verified permissions were granted via the Engineering share-access security group.
5. Checked user’s group membership in ADUC → user was missing from the required group.
6. Re-added user to the Engineering share-access group.
7. Refreshed user session and retested.

## Root Cause
User was mistakenly removed from the required **Engineering share-access security group** during access cleanup.

## Resolution
- Re-added `test-user-Eng` to the correct Engineering share-access group.
- User performed a fresh logon.
- Access to the share was restored.

## Validation
- User confirmed successful access to LAB-SHARE-01.
- Expected files and folders were visible.

## Full Visual Evidence
**[View complete Folge screenshot bundle](../../evidence/SDESK-3%20User%20reports%20Access%20Denied%20to%20share%20folder.pdf)**

## Lessons Learned
- Always use security groups for share/NTFS permissions (never direct user assignments).
- Group membership changes require a new logon session to take effect.
- Classic AGDLP troubleshooting pattern.

**Reference:** SDESK-3 (Folge export)  

**Related Files:**  
- `docs/architecture/ou-hierarchy.md` (AGDLP section)

**Status:** Resolved 13-May-2026