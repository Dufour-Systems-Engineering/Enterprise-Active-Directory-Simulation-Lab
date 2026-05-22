# Helpdesk Delegation Testing Results

**Date:** April 29–30, 2026  
**Purpose:** Validate least-privilege delegation using `HelpdeskNew` as the pilot account on the isolated Test OU.

## Test Scope
- Account: `HelpdeskNew` (member of `GG_HelpDesk_Users`)
- Target OU: `OU=Users,OU=Test,OU=Accounts,DC=lab,DC=local`

## Test Results

| Permission                    | Test OU Result | Other OUs Result | Status |
|-------------------------------|----------------|------------------|--------|
| Reset Password                | ✅ Success      | ✅ Access Denied | Pass   |
| Unlock Account                | ✅ Success      | ✅ Access Denied | Pass   |
| Modify User Properties        | ✅ Success      | ✅ Access Denied | Pass   |
| Create User                   | ✅ Success      | ✅ Access Denied | Pass   |
| Delete User                   | ✅ Success      | ✅ Access Denied | Pass   |
| Move User                     | ✅ Success      | ✅ Access Denied | Pass   |

## Important Scope Clarification
While the screenshots and testing in this document show `HelpdeskNew` successfully performing user creation, deletion, and moves, **these operations were conducted only during the pilot/testing phase**.  

In the **final production design**, **Create**, **Delete**, and **Move** permissions are reserved exclusively for `GG_IT_Admins`.  
`GG_HelpDesk_Users` was used as the test/pilot account to validate the delegation process and identify issues before applying strict least-privilege rules.

## Lessons Learned
- Delegation of Control Wizard produces cleaner ACLs than manual dsacls.
- Move operations require permissions on **both** source and target OUs.
- “Protect object from accidental deletion” must be disabled on source/target OUs for moves to succeed.
- Always perform a logoff/logon on the client after delegation changes.

## Related Files:
- [Delegation Overview](delegation-overview.md)
- [Delegate User Moves](delegate-user-moves.md)
- [Helpdesk Delegation Runbook](../runbooks/helpdesk-delegation-runbook.md)