# Helpdesk Delegation Runbook

**Last Updated:** May 21, 2026  
**Purpose:** Validate least-privilege delegation for Tier 1 Help Desk accounts.

## Scope
- Test Account: `HelpdeskNew` (member of `GG_HelpDesk_Users`)
- Positive Testing: Isolated `Test` OU
- Negative Testing: Production departmental OUs

## Prerequisites
- HelpdeskNew account created
- Delegation configured via GUI or dsacls
- WindowsADClient workstation available

## GUI Delegation Procedure

### Grant Permissions (Source OU)
1. Right-click source OU → **Properties** → **Security** → **Advanced**
2. Add `GG_HelpDesk_Users`
3. Applies to: **This object and all descendant objects**
4. Grant: **Delete child objects** (User class)

**Reference:** [Screenshot: Delegation of Control Wizard – Before](../../screenshots/delegations/delegate-control-Eng-before.png)

### Grant Permissions (Destination OU)
1. Right-click destination OU → **Properties** → **Security** → **Advanced**
2. Add `GG_HelpDesk_Users`
3. Applies to: **This object and all descendant objects**
4. Grant: **Create child objects** (User class)

**Reference:** [Screenshot: Advanced Security Settings – After (HelpdeskNew permissions)](../../screenshots/delegations/delegate-control-Eng-after.png)

### Clear Protection Flag
Uncheck **Protect object from accidental deletion** on:
- The user object
- Source OU
- Destination OU

**Reference:** [Screenshot: Engineering OU – Protection enabled (before)](../../screenshots/delegations/delegate-control-Eng-ou-protect-from-deletion-before.png)  
**Reference:** [Screenshot: Engineering OU – Protection disabled (after)](../../screenshots/delegations/delegate-control-Eng-ou-protect-from-deletion-after.png)  
**Reference:** [Screenshot: Users sub-OU – Protection enabled (before)](../../screenshots/delegations/delegate-control-Eng-protect-from-deletion-user-ou-before.png)  
**Reference:** [Screenshot: Users sub-OU – Protection disabled (after)](../../screenshots/delegations/delegate-control-Eng-protect-from-deletion-user-ou-after.png)

## GUI Move Procedure (Positive Test)
1. Log into `WindowsADClient` as `HelpdeskNew`.
2. Open Active Directory Users and Computers (`dsa.msc`).
3. Navigate to the source OU: `OU=Users,OU=Engineering,OU=Accounts,DC=lab,DC=local`.
4. Locate the test user (e.g. `TestUser.Eng` or `TestUser.Dele...`).
5. Right-click the user → **Move…** (or simply drag-and-drop).
6. Select the destination OU: `OU=Users,OU=TEST,OU=Accounts,DC=lab,DC=local`.
7. Click **OK**.

**Reference:**  
[Screenshot: Test user in Engineering\Users – Before](../../screenshots/delegations/delegate-control-Eng-move-user-before.jpeg)  
[Screenshot: Test user moved to TEST\Users – After](../../screenshots/delegations/delegate-control-Eng-move-user-after.jpeg)

## PowerShell Reference (for Admins)

```powershell
$ou = "OU=Users,OU=Test,OU=Accounts,DC=lab,DC=local"

# Reset existing permissions
dsacls $ou /R "LAB\HelpdeskNew"

# Grant Reset Password + Properties
dsacls $ou /G "LAB\GG_HelpDesk_Users:CA;Reset Password" /I:S
dsacls $ou /G "LAB\GG_HelpDesk_Users:WP;*" /I:S
dsacls $ou /G "LAB\GG_HelpDesk_Users:RP;*" /I:S
```

**Important:** Always log off and log back in on the client after changes.

## Test Results

### Positive Test (Pilot Workflow)
*Verification that the `HelpdeskNew` pilot account successfully executes delegated tasks within the isolated Test OU environment.*  
**Reference:** [Screenshot: Successful password reset in Test OU](../../screenshots/delegations/delegate-control-Eng-positive-test.png)

### Negative Test (Production Security Lockdown)
*Verification of least-privilege enforcement. Once pilot testing concluded and permissions were restricted to the final design, the Help Desk account was explicitly blocked from modifying production OUs.*  
**Reference:** [Screenshot: Access Denied on production user](../../screenshots/delegations/delegate-control-Eng-negative-pw-reset-test-example.png)

## Test Results Table

| Test                  | Test OU Result     | Production OU Result | Status |
|-----------------------|--------------------|----------------------|--------|
| Reset Password        | ✅ Success          | ✅ Access Denied     | Pass   |
| Unlock Account        | ✅ Success          | ✅ Access Denied     | Pass   |
| Create User           | ✅ Success          | ✅ Access Denied     | Pass   |
| Delete User           | ✅ Success          | ✅ Access Denied     | Pass   |
| Move User             | ✅ Success          | ✅ Access Denied     | Pass   |

**Note:** Create/Delete/Move rights are reserved for `GG_IT_Admins` in final design. Help Desk was used for piloting only.

## Related Files
- [Delegate User Moves](delegate-user-moves.md)
- [Delegation Overview](../delegation/delegation-overview.md)
---