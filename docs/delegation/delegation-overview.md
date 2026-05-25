# Delegation Overview & Strategy
**Last Updated:** May 24, 2026
**Domain:** lab.local

## Objective
This lab implements a **two-tier delegation model** for Help Desk and IT Admin users as part of a realistic mid-sized enterprise Active Directory simulation. The design aligns with AGDLP principles and focuses on least-privilege access to support practical learning in delegated administration.

## Delegation Model

### Help Desk (Tier 1)
- **Group**: `GG_HelpDesk_Users`
- **Scope**: Departmental `Users` OUs (Sales, Engineering, Finance, HR)
- **Rights**: Password reset (with force change at next logon), account unlock, and read/write user properties.

### IT Admins (Tier 2)
- **Group**: `GG_IT_Admins`
- **Scope**: Departmental `Users` OUs plus privileged OUs (Help Desk, IT Admins, Managers)
- **Rights**: Full Control (`GenericAll`) on target OUs and descendant objects.

**Design Philosophy**: Tier 1 is intentionally limited to common daily support tasks. Tier 2 provides the elevated rights necessary for structural changes and bulk operations. Global groups handle user membership and are nested into Domain Local groups for clean, scalable permission assignment.

## Current Implementation State
The two-tier model is fully operational across main departmental OUs. Delegation was implemented using the Active Directory Delegation of Control Wizard supplemented by PowerShell for precise ACL control. An isolated Test OU was used extensively for safe validation before applying changes to production OUs.

**Current Limitations**: Manager-level self-service delegation and granular attribute-level controls have not yet been implemented. The existing foundation is structured to support these expansions.

This implementation demonstrates core enterprise delegation concepts suitable for learning and portfolio purposes.

## Related Files
- [Helpdesk Delegation Testing Results](helpdesk-delegation-testing-results.md)
- [Future Delegation Expansion](future-delegation-expansion.md)