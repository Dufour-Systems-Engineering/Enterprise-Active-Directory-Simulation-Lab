# Group Management Runbook

**Last Updated:** May 22, 2026  
**Purpose:** Complete reference for the design, creation, population, nesting, and management of security groups in the `lab.local` domain using the AGDLP model.

## 1. Final Group Strategy (AGDLP)

The lab implements Microsoft’s recommended **AGDLP** nesting model for scalable, least-privilege access control:

- **A**ccounts → User objects placed in departmental OUs
- **G**lobal Groups (`GG_`) → Contain users by department or role
- **D**omain **L**ocal Groups (`DL_`) → Used for resource permissions (planned for future shares)
- **P**ermissions → Assigned to Domain Local groups

**Group Location:** All security groups are stored in `OU=Groups,DC=lab,DC=local`

**Standardized Naming Convention**
- Global Groups: `GG_<Department>_Users`
- Domain Local Groups: `DL_<Department>_<Permission>`

**Groups Created**
- `GG_Engineering_Users`
- `GG_Sales_Users`
- `GG_Finance_Users`
- `GG_HR_Users`
- `GG_HelpDesk_Users`
- `GG_IT_Admins`
- `GG_Managers`
- `GG_Misc_Users`

**Reference:**  
[Screenshot: Global Security Group Creation](../../screenshots/group-creation-&-management/global-security-group-creation-ps.png)

## 2. Design Decision-Making Process

Key decisions made during the build:
- Chose the **AGDLP** model after researching Microsoft best practices for enterprise environments.
- Decided to separate **OUs** (for structure and GPO targeting) from **Security Groups** (for permissions and access control).
- Created one Global Group per department to hold actual users.
- Placed all groups in a dedicated top-level `OU=Groups` rather than inside departmental OUs.
- Expanded from initial departments to include `IT_Admins`, `Managers`, `HelpDesk`, and `Misc`.
- Goal was explicitly portfolio-focused: to demonstrate understanding of real enterprise group strategy rather than a basic lab setup.

## 3. Creating Security Groups

### PowerShell Method (Recommended for Scale)

```powershell
$gggGroups = @(
    "GG_Sales_Users", "GG_Engineering_Users", "GG_Finance_Users",
    "GG_HR_Users", "GG_HelpDesk_Users", "GG_IT_Admins",
    "GG_Managers", "GG_Misc_Users"
)

foreach ($group in $gggGroups) {
    New-ADGroup -Name $group `
                -GroupScope Global `
                -GroupCategory Security `
                -Path "OU=Groups,DC=lab,DC=local" `
                -Description "Global group for department users"
}
```

**Reference:**  
[Screenshot: Global Security Group Creation](../../screenshots/group-creation-&-management/global-security-group-creation-ps.png)

### Creating Domain Local Groups

```powershell
$dlGroups = @(
    "DL_Sales_Access", "DL_Engineering_Access", "DL_Finance_Access",
    "DL_HR_Access", "DL_HelpDesk_Access", "DL_Managers_Access"
)

foreach ($group in $dlGroups) {
    New-ADGroup -Name $group `
                -GroupScope DomainLocal `
                -GroupCategory Security `
                -Path "OU=Groups,DC=lab,DC=local" `
                -Description "Domain Local group for resource permissions"
}
```

**Reference:**  
[Screenshot: Domain Local Security Group Creation](../../screenshots/group-creation-&-management/domain-local-security-group-ceation-ps.png)

## 4. Populating Groups with Users

```powershell
$departments = @{
    "Engineering" = "GG_Engineering_Users"
    "Sales"       = "GG_Sales_Users"
    "Finance"     = "GG_Finance_Users"
    "HR"          = "GG_HR_Users"
    # ... other departments
}

foreach ($dept in $departments.Keys) {
    $groupName = $departments[$dept]
    $ouPath    = "OU=Users,OU=$dept,OU=Accounts,DC=lab,DC=local"
    $users     = Get-ADUser -Filter * -SearchBase $ouPath

    if ($users) {
        Add-ADGroupMember -Identity $groupName -Members $users
        Write-Host "Added $($users.Count) users to $groupName" -ForegroundColor Green
    }
}
```

**Reference:**  
[Screenshot: Adding Users to Global Security Groups](../../screenshots/group-creation-&-management/add-group-members-to-global-sec-groups-ps.png)

## 5. Nesting Global Groups into Domain Local Groups (AGDLP)

```powershell
$nesting = @{
    "GG_Sales_Users"       = "DL_Sales_Access"
    "GG_Engineering_Users" = "DL_Engineering_Access"
    "GG_Finance_Users"     = "DL_Finance_Access"
    "GG_HR_Users"          = "DL_HR_Access"
    "GG_HelpDesk_Users"    = "DL_HelpDesk_Access"
    "GG_Managers"          = "DL_Managers_Access"
}

foreach ($gg in $nesting.Keys) {
    $dl = $nesting[$gg]
    Add-ADGroupMember -Identity $dl -Members $gg
    Write-Host "Nested $gg → $dl" -ForegroundColor Green
}
```

**Reference:**  
[Screenshot: Nesting GG into DL Groups](../../screenshots/group-creation-&-management/addlp-gg-nested-in-dl-ps.png)  
[Screenshot: Global Groups Nested into Domain Local](../../screenshots/group-creation-&-management/global-groups-nesting-to-domain-local-ps.png)

## 6. Verification

```powershell
# View members of a group
Get-ADGroupMember -Identity "GG_Engineering_Users" | Select Name, SamAccountName
```

## 7. Troubleshooting (Real Issues Encountered)

| Issue | Cause | Resolution |
|-------|-------|------------|
| User added to group but permissions not applying | Cached Kerberos token | User must log off and log back in (`klist purge`) |
| Group membership not updating | Replication delay or token caching | Force logoff/logon + `gpupdate /force` |

## Lessons Learned
- AGDLP provides excellent separation of “who” (Global) from “what they can access” (Domain Local).
- PowerShell is far more efficient than GUI for creating and populating dozens of groups.
- Documenting every step (creation → population → nesting) makes the portfolio much stronger.

**Related Files**  
*(To be finalized in the final sweep)*
- [OU Management Runbook](ou-management-runbook.md)
- [Bulk User Import Runbook](bulk-user-import.md)
- [Helpdesk Delegation Runbook](helpdesk-delegation-runbook.md)
- [AGDLP Design](../architecture/agdlp-design.md)
