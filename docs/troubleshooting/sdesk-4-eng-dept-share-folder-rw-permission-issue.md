# Multiple Engineering Users Unable to Modify Share Folder (SDESK-4)

**Last Updated:** May 13, 2026  
**Purpose:** Document the resolution of an NTFS permissions issue preventing Engineering users from creating or modifying files in the department share.

## Lab Context

| Item              | Value                                      |
|-------------------|--------------------------------------------|
| Domain            | lab.local                                  |
| Ticket            | SDESK-4                                    |
| Affected Users    | Multiple Engineering users                 |
| Department        | Engineering                                |
| Share Server      | LAB-SHARE-01                               |
| Client            | WindowsADClient                            |
| Issue Type        | NTFS Permissions (Modify Access)           |

## Problem Summary
Multiple Engineering users could access the `LAB-SHARE-01` shared folder but were unable to create new folders or modify contents.  
Error: **“Destination Folder Access Denied”**

## Resolution Steps

1. Verified users could browse the share but could not create or modify items.
2. Confirmed the issue affected multiple Engineering users.
3. Reviewed Share permissions on `LAB-SHARE-01` (correct).
4. Reviewed NTFS permissions on the target folder.
5. Identified that NTFS permissions for the Engineering group had been reduced to Read & List only.
6. Restored NTFS permissions to the required **Modify** level for the Engineering group.
7. Retested file creation and modification.

## Root Cause
Recent permissions cleanup inadvertently lowered the **NTFS permissions** for the Engineering department group (Read/List only instead of Modify).

## Validation
- Users confirmed they could now create, edit, and delete content in the share.
- Full read/write functionality restored.

## Visual Evidence
**Full walkthrough available in:**  
`screenshots/sdesk-4-multiple-engineering-users-access-denied/`

**Key Screenshots:**
- [01-user-unable-to-create-folder.png](../../screenshots/sdesk-4-multiple-engineering-users-access-denied/01-user-unable-to-create-folder.png) — Destination Folder Access Denied error
- [02-ntfs-vs-share-permissions.png](../../screenshots/sdesk-4-multiple-engineering-users-access-denied/02-ntfs-vs-share-permissions.png) — NTFS permissions differing from Share permissions
- [03-successful-folder-creation-modify.png](../../screenshots/sdesk-4-multiple-engineering-users-access-denied/03-successful-folder-creation-modify.png) — Successful folder creation and modification after fix

## Lessons Learned
- Share permissions control initial access, while **NTFS permissions** control actual read/write/modify capabilities.
- Always verify **both** Share and NTFS permissions when users can see a folder but cannot modify it.
- Document operational permission baselines after any cleanup activities.

**Related Files:**
- [User Reports Access Denied to Department File Share (SDESK-3)](../troubleshooting/sdesk-3-ad-share-access-denied.md)
- [Group Management Runbook](../runbooks/group-management-runbook.md)

**Status:** Resolved