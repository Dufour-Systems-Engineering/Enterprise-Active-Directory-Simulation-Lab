# Helpdesk Delegation Testing Results
**Date:** April 29 – May 4, 2026
**Purpose:** Validate the two-tier delegation model using `HelpdeskNew` (member of `GG_HelpDesk_Users`) as the pilot account, primarily on the isolated Test OU.

## Test Results
| Permission                  | Test OU Result      | Other OUs Result     | Status |
|-----------------------------|---------------------|----------------------|--------|
| Reset Password              | ✅ Success          | ✅ Access Denied     | Pass   |
| Unlock Account              | ✅ Success          | ✅ Access Denied     | Pass   |
| Modify User Properties      | ✅ Success          | ✅ Access Denied     | Pass   |
| Create User                 | ✅ Success (Pilot)  | ✅ Access Denied     | Pass   |
| Delete User                 | ✅ Success (Pilot)  | ✅ Access Denied     | Pass   |
| Move User Between OUs       | ✅ Success (Pilot)  | ✅ Access Denied     | Pass   |

## Pilot vs. Final Scope
Early testing temporarily granted broader permissions on the pilot account to validate core mechanics. In the final implementation, `GG_HelpDesk_Users` is restricted to password reset, unlock, and property modification. Create, delete, and move operations are reserved exclusively for `GG_IT_Admins`.

## Operational Lessons Learned
- The Delegation of Control Wizard produces cleaner, more maintainable ACLs than manual PowerShell ACE construction.
- Cross-OU moves require permissions on both source and target OUs.
- "Protect from accidental deletion" must be disabled on relevant containers for moves to succeed.
- Delegation changes often require session refresh (`runas` or logoff/logon) to take effect.
- The isolated Test OU was critical for safely identifying edge cases.

These results confirm the two-tier model performs reliably and highlight important operational realities of delegated administration.

## Related Files
- [Delegation Overview & Strategy](delegation-overview.md)