# OU Hierarchy Design

**Domain:** lab.local  
**Last Updated:** May 18, 2026

## Overview

The organizational unit structure was designed to emulate a realistic small-to-mid-sized enterprise environment. It provides clear departmental separation, supports delegated administration, enables targeted Group Policy application, and includes isolated testing capabilities while maintaining scalability for approximately 1,000 users.

## Current OU Hierarchy

```
lab.local
├── Accounts
│   ├── Engineering
│   │   └── Users
│   ├── Finance
│   │   └── Users
│   ├── HelpDesk
│   │   └── Users
│   ├── HR
│   │   └── Users
│   ├── IT_Admins
│   │   └── Users
│   ├── Managers
│   │   └── Users
│   ├── Misc
│   │   └── Users
│   ├── Sales
│   │   └── Users
│   └── TEST
│       ├── Computers
│       ├── Groups
│       └── Users
├── Groups
├── Workstations
└── Servers
```

## Design Rationale

### 1. Top-Level Accounts OU
All user accounts are centralized under a single `OU=Accounts`.  
**Reasoning:** Creates one logical container for people, simplifying management, auditing, and delegation.

### 2. Department OU + Users Sub-OU Pattern
Each department has its own OU with a dedicated `Users` sub-OU.  
**Reasoning:** 
- Provides clean granularity for department-specific GPOs and delegations.
- Allows future expansion (e.g., Managers vs regular staff) without disruptive restructuring.
- Keeps permission targeting predictable and maintainable.

### 3. Role-Based Department OUs
HelpDesk, IT_Admins, and Managers each received their own department-level OU under Accounts.  
**Reasoning:** These roles have unique permission and policy requirements that differ significantly from standard business users. Treating them as distinct departments simplifies role-based GPO application and delegation.

### 4. Isolated TEST OU
The TEST OU sits at the same level as the other departments and contains its own `Computers`, `Groups`, and `Users` sub-OUs.  
**Reasoning:** Isolates testing and break/fix scenarios to limit blast radius while still allowing realistic departmental testing.

### 5. Dedicated Groups OU
All security groups reside in a dedicated `OU=Groups` at the domain root.  
**Reasoning:** Keeps group management centralized and prevents the Accounts OU from becoming cluttered with both users and groups.

### 6. Computer OUs
Separate `Workstations` and `Servers` OUs exist at the domain root.  
**Reasoning:** Enables targeted Group Policy application for client devices and servers independently of user OUs.

---

This hierarchy reflects deliberate decisions focused on scalability, operational clarity, least-privilege delegation, and real-world enterprise practices.

---