# OU Management Runbook

**Last Updated:** May 20, 2026  
**Purpose:** Comprehensive reference for the complete design, creation, management, protection, renaming, migration, and troubleshooting of Organizational Units in the `lab.local` domain.

## 1. Final OU Hierarchy


```

lab.local
├── Accounts
│   ├── Engineering
│   │   └── Users
│   ├── Sales
│   │   └── Users
│   ├── Finance
│   │   └── Users
│   ├── HR
│   │   └── Users
│   ├── HelpDesk
│   │   └── Users
│   ├── IT_Admins
│   │   └── Users
│   ├── Managers
│   │   └── Users
│   └── Misc
│       └── Users
├── Test (Isolated testing OU)
│   ├── Users
│   ├── Computers
│   └── Groups
├── Groups
├── Workstations
└── Servers

```

> **Directory Architecture Reference:** > For a full operational snapshot of the expanded Active Directory tree layout, see the environment blueprint: [01-ou-hierarchy-full.png](screenshots/ou-management/01-ou-hierarchy-full.png).

---

## 2. Design Decision-Making Process

The OU structure was deliberately designed to emulate a realistic small-to-mid-sized enterprise environment. Key decisions included:

- Adoption of the **Account OU → Department → Users** pattern to enable clean delegation and GPO targeting.
- Creation of a dedicated `Users` sub-OU under every department (instead of placing users directly in the department OU) for better organization and future-proofing.
- Standardization of department names for professionalism (`Help_Desk` → `HelpDesk`, `It_Admins` → `IT_Admins`, `WorkStations` → `Workstations`).
- Addition of separate OUs for `IT_Admins`, `Managers`, `HelpDesk`, and `Misc` after initial planning.
- Creation of an isolated `Test` OU for safe delegation and break-fix testing.

---

## 3. Creating OUs

### GUI Method
1. Open `dsa.msc`
2. Right-click parent container → **New** → **Organizational Unit** * *See Evidence:* [02-creating-ou-gui.png](screenshots/ou-management/02-creating-ou-gui.png)
3. Enter name
4. Check **Protect object from accidental deletion** (recommended for production)  
   * *See Evidence:* [03-protect-from-deletion-checkbox.png](screenshots/ou-management/03-protect-from-deletion-checkbox.png)

### PowerShell Method
```powershell
# Create department OU
New-ADOrganizationalUnit -Name "Engineering" `
                         -Path "OU=Accounts,DC=lab,DC=local" `
                         -ProtectedFromAccidentalDeletion $true

# Create Users sub-OU
New-ADOrganizationalUnit -Name "Users" `
                         -Path "OU=Engineering,OU=Accounts,DC=lab,DC=local" `
                         -ProtectedFromAccidentalDeletion $true

```

---

## 4. Renaming OUs

```powershell
Rename-ADObject -Identity "OU=Help_Desk,OU=Accounts,DC=lab,DC=local" -NewName "HelpDesk"
Rename-ADObject -Identity "OU=It_Admins,OU=Accounts,DC=lab,DC=local" -NewName "IT_Admins"

```

> **Object Modification Logs:** > The workflow state immediately preceding and following a container rename can be reviewed at [04-rename-ou-before.png](https://www.google.com/search?q=screenshots/ou-management/04-rename-ou-before.png) and [05-rename-ou-after.png](https://www.google.com/search?q=screenshots/ou-management/05-rename-ou-after.png).

---

## 5. Protect from Accidental Deletion ("The Three Locks")

This flag was the #1 cause of "Access is denied" errors during object moves. Objects must be unlocked across multiple scopes to safely complete a migration.

```powershell
# Disable protection on target OU
Set-ADOrganizationalUnit -Identity "OU=Users,OU=TEST,OU=Accounts,DC=lab,DC=local" -ProtectedFromAccidentalDeletion $false

# Disable on user object
Set-ADObject -Identity "CN=TestMove,OU=Users,OU=TEST,OU=Accounts,DC=lab,DC=local" -ProtectedFromAccidentalDeletion $false

```

> **Object Migration Tracking:** > For step-by-step verification logs of moving active directory security principals across paths, see [06-move-object-before.png](https://www.google.com/search?q=screenshots/ou-management/06-move-object-before.png) and [07-move-object-after.png](https://www.google.com/search?q=screenshots/ou-management/07-move-object-after.png).

---

## 6. Verification

To verify that all structural OUs have populated correctly and audit their respective deletion protection values domain-wide, run the following pipe:

```powershell
Get-ADOrganizationalUnit -Filter * | 
  Select Name, DistinguishedName, ProtectedFromAccidentalDeletion | 
  Sort DistinguishedName | Format-Table -AutoSize

```

> **Automated Audit Output:** > The table output returned by this verification command is logged at [08-verification-get-adou.png](https://www.google.com/search?q=screenshots/ou-management/08-verification-get-adou.png).

---

## 7. Troubleshooting (Real Issues Encountered)

| Issue | Cause | Resolution |
| --- | --- | --- |
| "Access is denied" on move | ProtectedFromAccidentalDeletion enabled on source, destination, or object | Disable flag on all three layers ("The Three Locks") |
| Old OU remained after rename | Rename does not delete old container | Manually delete after disabling protection |
| Move still fails after clearing flags | Missing Delete/Create Child rights | Re-run Delegation of Control Wizard |
| Cannot delete OU | Child objects present | Use `-Recursive` flag after disabling protection |

### Multi-Lock Exception Analysis

When resolving deep permission inheritance and accidental deletion bugs during cross-container operations, use the administrative property sheets to verify the scope blocks:

* **Source/Object Locking States:** See [09a-three-locks-troubleshooting.png](https://www.google.com/search?q=screenshots/ou-management/09a-three-locks-troubleshooting.png)
* **Active Security Descriptors:** See [09b-three-locks-troubleshooting.png](https://www.google.com/search?q=screenshots/ou-management/09b-three-locks-troubleshooting.png)
* **Destination Boundary Audits:** See [09c-three-locks-troubleshooting.png](https://www.google.com/search?q=screenshots/ou-management/09c-three-locks-troubleshooting.png)

---

## Related Files

* [Domain Join Runbook](https://www.google.com/search?q=domain-join-runbook.md)
* [Group Management Runbook](https://www.google.com/search?q=group-management-runbook.md)
* [Delegation Overview](https://www.google.com/search?q=../delegation/delegation-overview.md)
* [GPO Overview](https://www.google.com/search?q=../gpo/gpo-overview.md)