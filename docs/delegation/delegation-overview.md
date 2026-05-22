# Delegation Overview & Strategy

**Last Updated:** May 19, 2026  
**Domain:** lab.local

## Objective
Implement least-privilege delegation following AGDLP best practices:
- Limited Tier 1 rights for Help Desk
- Broader administrative rights for IT Admins

## Delegation Model

### 1. Help Desk (Tier 1) – Limited Rights
- Group: `GG_HelpDesk_Users`
- Applied to: Departmental `Users` OUs
- Rights Granted:
  - Reset password + force change at next logon
  - Read and write all user properties
  - Unlock user accounts

**Important Note on Scope:**  
Although some screenshots and testing (particularly KAN-98) show HelpdeskNew performing user moves and deletes, these operations were executed **only during the pilot/testing phase**. In the final design, **Create and Delete** permissions are reserved exclusively for `GG_IT_Admins`. Help Desk was intentionally used as the test account to validate the delegation process before tightening the scope.

### 2. IT Admins – Broad Rights
- Group: `GG_IT_Admins`
- Applied to: Departmental `Users` OUs
- Rights Granted:
  - Full Control on departmental `Users` OUs
  - Full ability to create, delete, and move user objects

**Purpose:** Enable senior administrators to perform escalated tasks while keeping Help Desk strictly limited.

## Key Design Decisions

- Delegation of Control Wizard was used for cleaner ACLs.
- Permissions scoped at the departmental `Users` OU level.
- "Protect object from accidental deletion" handling documented as a prerequisite for moves.
- Positive testing in isolated Test OU; negative testing in production departments.

## Related Files
-[Helpdesk Delegation Testing Results](helpdesk-delegation-testing-results.md)
- [Delegate User Moves](delegate-user-moves.md)
- [Helpdesk Delegation Runbook](../runbooks/helpdesk-delegation-runbook.md)

---