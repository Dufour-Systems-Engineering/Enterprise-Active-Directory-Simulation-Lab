# Delegate User Moves Between OUs

**Last Updated:** May 21, 2026  
**Purpose:** Document the least-privilege delegation process and testing for moving user objects between Organizational Units in the `lab.local` domain.

## Business Rationale
Documenting user-move delegation was critical because:
- It demonstrates real-world application of the AGDLP model and least-privilege principles.
- It validates that Help Desk accounts can perform limited administrative tasks without full Domain Admin rights.
- The "Three Locks" troubleshooting (ProtectedFromAccidentalDeletion flags) was a common pain point that needed clear evidence and resolution steps.

## Prerequisites
- Domain Administrator rights to configure delegation
- Target delegated account (e.g. `HelpdeskNew` in `GG_HelpDesk_Users`)
- Advanced Features enabled in Active Directory Users and Computers (`dsa.msc`)

## Procedure (GUI Method)

### 1. Grant Permissions on Source OU
1. Right-click the **source OU** → **Properties** → **Security** tab → **Advanced**
2. Click **Add** → select the principal (e.g. `GG_HelpDesk_Users`)
3. Set **Applies to**: `This object and all descendant objects`
4. Grant **Delete child objects** (for User class)

### 2. Grant Permissions on Destination OU
1. Right-click the **destination OU** → **Properties** → **Security** tab → **Advanced**
2. Click **Add** → select the principal
3. Set **Applies to**: `This object and all descendant objects`
4. Grant **Create child objects** (for User class)

### 3. Clear Protect from Accidental Deletion ("Three Locks")
Uncheck this flag on:
- The user object being moved
- The source OU
- The destination OU

### 4. Perform the User Move (Positive Test)
1. Log into `WindowsADClient` as the delegated account (`HelpdeskNew`).
2. Open **Active Directory Users and Computers**.
3. Navigate to the source OU (`OU=Users,OU=Engineering,OU=Accounts,DC=lab,DC=local`).
4. Locate the test user (e.g. `TestUser.Dele...`).
5. Right-click the user → **Move…** (or drag-and-drop).
6. Select the destination OU (`OU=Users,OU=TEST,OU=Accounts,DC=lab,DC=local`).
7. Click **OK**.

**Reference:**  
[Screenshot: Test user in Engineering\Users – Before move](../../screenshots/delegations/delegate-control-Eng-move-user-before.jpeg)  
[Screenshot: Test user successfully moved to TEST\Users – After move](../../screenshots/delegations/delegate-control-Eng-move-user-after.jpeg)

## Important Scope Note
Help Desk accounts were used during testing/piloting only. In the final design, **Create/Delete/Move** permissions are reserved exclusively for `GG_IT_Admins`.

## Test Results

| Test Case | Protect Flag Status       | Result        | Notes                  |
|-----------|---------------------------|---------------|------------------------|
| T-01      | Enabled on user           | Access Denied | Expected               |
| T-02      | Enabled on OU             | Access Denied | Expected               |
| T-03      | Disabled on both          | Success       | Move completed         |

## Lessons Learned
- The "Three Locks" (ProtectedFromAccidentalDeletion on user + source OU + destination OU) is the most common cause of "Access Denied" during object moves.
- Always test in an isolated `TEST` OU first before applying to production departmental OUs.
- GUI delegation is excellent for learning and documentation; `dsacls` is preferred for repeatability in production.

**Related Files**  
*(To be finalized in the final sweep)*
- [Helpdesk Delegation Runbook](helpdesk-delegation-runbook.md)
- [Helpdesk Delegation Testing Runbook](../delegation/helpdesk-delegation-testing.md)
- [OU Management Runbook](../runbooks/ou-management-runbook.md)
```

---
