# AD DS Break/Fix: Multiple Engineering Users Report Access Denied to Shared Folder (Cannot Modify) (sdesk-4)

## Lab Context

| Item              | Value                                      |
|-------------------|--------------------------------------------|
| Domain            | lab.local                                  |
| Ticket            | SDESK-4                                    |
| Affected Users    | Multiple Engineering users                 |
| Department        | Engineering                                |
| Share Server      | LAB-SHARE-01                               |
| Client            | WindowsADClient                            |
| Issue Type        | NTFS permissions (modify access)           |

## Problem Summary
Multiple Engineering users could open the `LAB-SHARE-01` shared folder but were unable to modify its contents (create, edit, or delete files).  
Error: **“Destination Folder Access Denied”**

## Troubleshooting Steps
1. Verified users could access the folder but could not modify files.
2. Confirmed the issue affected the entire Engineering department.
3. Verified correct group membership for the Engineering share-access group.
4. Reviewed Share permissions on LAB-SHARE-01 (correct).
5. Reviewed NTFS permissions on the target folder.
6. Identified that NTFS permissions for the Engineering group had been reduced to **Read & List contents only** during a recent permissions cleanup.
7. Restored NTFS permissions to the operational **Modify** baseline.
8. Retested file creation and modification successfully.

## Root Cause
Recent folder permission cleanup inadvertently restricted the **NTFS permissions** for the Engineering department group below the intended operational baseline (Read/List only instead of Modify).

## Resolution
- Updated NTFS permissions on the target shared folder to restore **Modify** access for the Engineering group.
- No changes needed to Share permissions or group membership.

## Validation
- Users confirmed they could now access **and modify** files in the shared folder.
- Successfully tested file creation and editing.

## Full Visual Evidence
**[View complete Folge screenshot bundle](../docs/troubleshooting/evidence/[#SDESK-4]%20Multiple%20Engineering%20users%20report%20Access%20Denied%20to%20shared%20folder.pdf)**

## Lessons Learned
- Share permissions control initial access, but **NTFS permissions** control actual read/write/modify rights.
- Always verify both Share and NTFS permissions when users can see a folder but cannot modify it.
- Document and test operational baselines after any permission cleanup.

**Reference:** SDESK-4 (Folge export)  

**Related Files:**  
- `sdesk-3-share-access-denied.md`

**Status:** Resolved 14-May-2026