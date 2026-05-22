# AGDLP Design Strategy

**Domain:** lab.local  
**Last Updated:** May 18, 2026

This document details the deliberate decision to implement Microsoft’s **AGDLP** group model throughout the lab environment.

## Why AGDLP?

AGDLP (Accounts → Global → Domain Local → Permissions) is Microsoft’s recommended best-practice model for group nesting in Active Directory. I chose this model from the very beginning for the following reasons:

- To build the lab using recognized enterprise standards rather than shortcuts or simplified approaches.
- To enforce **least-privilege** principles consistently.
- To create clear separation between “who” (Global groups) and “what they can access” (Domain Local groups).
- To future-proof the environment for easier scaling, auditing, and GPO application.
- To demonstrate that even without prior corporate Active Directory experience, I could research, understand, and apply industry best practices.

## Implementation in This Lab

### 1. Accounts (A)
User accounts are placed in departmental OUs under the top-level `OU=Accounts`.

### 2. Global Groups (G)
Departmental and role-based Global groups (`GG_`) contain the user accounts.

**Examples:**
- `GG_Engineering_Users`
- `GG_Sales_Users`
- `GG_Finance_Users`
- `GG_HR_Users`
- `GG_HelpDesk_Users`
- `GG_IT_Admins`
- `GG_Managers`

### 3. Domain Local Groups (DL)
Permissions and GPO security filtering are assigned exclusively to Domain Local groups (`DL_`).

**Examples:**
- `DL_Engineering_Access`
- `DL_Sales_Access`
- `DL_Finance_Access`
- `DL_HelpDesk_Access`

### 4. Permissions (P)
All permissions (NTFS shares, GPO delegation, resource access, etc.) are granted to Domain Local groups only — never directly to users or Global groups.

## Key Benefits Realized

- **Granularity** — Allows precise targeting of GPOs and permissions without affecting entire departments.
- **Scalability** — Adding or removing users requires only group membership changes; permissions remain untouched.
- **Administrative Clarity** — The `GG_` and `DL_` prefixes instantly communicate scope and purpose.
- **Least Privilege** — Permissions are applied at the most appropriate level.
- **Easier Troubleshooting** — Group membership and permission assignments are logical and easy to audit.

This implementation reflects a conscious effort to build the lab as a realistic, production-like environment rather than a minimal proof-of-concept.

---