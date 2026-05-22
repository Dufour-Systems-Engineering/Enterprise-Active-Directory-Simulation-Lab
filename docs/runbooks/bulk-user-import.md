# Bulk User Import Runbook
**Last Updated:** May 21, 2026
**Domain:** lab.local
**Purpose:** Automate the creation of ~1,000 realistic test users across departmental OUs to simulate a mid-sized enterprise environment for GPO, delegation, and break/fix testing.

## Executive Summary / Business Rationale
Importing nearly 1,000 users was a deliberate strategic choice to elevate the lab from a basic “few users in one OU” setup to a realistic enterprise-scale environment. This allowed meaningful testing of Group Policy inheritance, AGDLP group nesting, delegation, and cross-OU operations at production-like volume.

* **Baseline Bulk Import:** 958 users successfully created and placed in the correct departmental `Users` sub-OUs under `OU=Accounts`.
* **Active Lab Lifecycle (Post-Import Testing):** 971 total users. The additional 13 accounts were intentionally provisioned downstream during manual delegation verification, service account testing, and GPO baseline validation filtering scenarios.

---
## Design Decision – User Volume & Departmental Detail
The decision to import a large number of users (~1,000) with realistic first/last names, departments, job titles, and properly formatted attributes was made **to simulate to the best of my ability a mid-level corporate environment**. 

A small test set (10–50 users) would not have provided enough scale or complexity to meaningfully test GPO targeting, security filtering, delegation boundaries, or bulk administration scenarios. By using real department names (Engineering, Sales, Finance, HR, Help Desk, IT_Admins, Managers, Misc) and professional-looking attributes, the lab more closely mirrors what an administrator would encounter in an actual mid-sized company. This approach strengthens the portfolio by demonstrating enterprise-scale thinking rather than a minimal lab exercise.

---
## Mockaroo Schema Used
| Field | Mockaroo Type / Formula | Purpose / AD Mapping |
|--------------------|--------------------------------------------------------------|-----------------------------------------------|
| first_name | First Name (built-in) | GivenName |
| last_name | Last Name (built-in) | Surname |
| sAMAccountName | `lower(left(first_name, 1) + last_name).gsub(/\W/, '')[0..19]` | Pre-Windows 2000 login ID (max 20 chars) |
| userprincipalname | `field('sAMAccountName') + "@lab.local"` | Full UPN |
| password | Fixed compliant string (`P@ssword123!`) | Meets domain GPO requirements |
| department | Custom list (HR, Help Desk, Sales, Engineering, etc.) | Department attribute |
| job_title | Job Title (built-in) | Title attribute |

*See Evidence:* [01-mockaroo-raw-csv-schema.png](../../screenshots/bulk-user-import/01-mockaroo-raw-csv-schema.png)

---
## Lab Context
| Item | Value |
|------------------------|---------------------------------------------|
| Target Baseline Count | ~1,000 (958 successfully created) |
| Active Lifecycle Count | 971 (Includes manual test accounts) |
| Source File | `C:\ADUsers.csv` (exported from Mockaroo) |
| Target OUs | Departmental `Users` sub-OUs under `OU=Accounts` |
| Method | PowerShell with data cleansing |

---
## Final Import Script (Production Version)
```powershell
$users = Import-Csv -Path "C:\ADUsers.csv"
foreach ($user in $users) {
    $cleanSam = ($user.sAMAccountName -replace " ", "").Trim()
    if ($cleanSam.Length -gt 20) {
        $cleanSam = $cleanSam.Substring(0,20)
    }
    $userParams = @{
        Name = "$($user.first_name) $($user.last_name)"
        GivenName = $user.first_name
        Surname = $user.last_name
        SamAccountName = $cleanSam
        UserPrincipalName = $user.userprincipalname
        Department = $user.department
        Title = $user.job_title
        AccountPassword = (ConvertTo-SecureString $user.password -AsPlainText -Force)
        Enabled = $true
        ChangePasswordAtLogon = $true
        Path = $user.OU
        ErrorAction = "Stop"
    }
    try {
        New-ADUser @userParams
        Write-Host "Success: $($user.userprincipalname)" -ForegroundColor Green
    }
    catch {
        Write-Warning "Failed to create $($user.first_name) $($user.last_name): $($_.Exception.Message)"
    }
}
```

*See Evidence:* [02-powershell-ise-staging.png](../../screenshots/bulk-user-import/02-powershell-ise-staging.png) and [03-script-execution-baseline.png](../../screenshots/bulk-user-import/03-script-execution-baseline.png)

---
## Verification
To audit the active directory baseline scale and confirm all accounts are successfully loaded within the root lab container, execute the following script command:
```powershell
(Get-ADUser -Filter * -SearchBase "OU=Accounts,DC=lab,DC=local").Count
```

*See Evidence:* [04-verification-user-count-971.png](../../screenshots/bulk-user-import/04-verification-user-count-971.png)

*See Evidence:* [05-verification-console-stream-01.png](../../screenshots/bulk-user-import/05-verification-console-stream-01.png) and [06-verification-console-stream-02.png](../../screenshots/bulk-user-import/06-verification-console-stream-02.png)

---
## Common Issues Encountered & Resolutions
| Issue | Cause | Resolution |
| --- | --- | --- |
| CSV column case sensitivity | `sAMAccountName` vs `samaccountname` | Use exact header casing |
| sAMAccountName > 20 characters | Long concatenated names | `[0..19]` slice + script cleansing |
| Special characters in names | Spaces, hyphens, apostrophes | `.gsub(/\W/, '')` + script cleanup |
| Password policy violations | Random Mockaroo passwords | Fixed compliant password string |
| Path String Exceptions (OUs) | Missing directory structural prefixes | Dynamically build distinguished names inside loop |

### Troubleshooting Archive (Artifact Logs)
* **Syntax Parameter Exceptions:** [07-error-log-password-syntax.png](../../screenshots/bulk-user-import/07-error-log-password-syntax.png)
* **Collision Exceptions:** [08-error-log-object-collision.png](../../screenshots/bulk-user-import/08-error-log-object-collision.png) and [09-error-log-upn-uniqueness.png](../../screenshots/bulk-user-import/09-error-log-upn-uniqueness.png)
* **Identity Scope Conflicts:** [09-error-log-upn-uniqueness.png](../../screenshots/bulk-user-import/09-error-log-upn-uniqueness.png)
* **Dynamic Path Correction Logic:** [10-dynamic-ou-path-cleansing.png](../../screenshots/bulk-user-import/10-dynamic-ou-path-cleansing.png)

---
## Lessons Learned
* Data cleansing before import is critical — small CSV issues explode at scale.
* Realistic volume + departmental structure makes GPO and delegation testing far more valuable.
* Proper planning and validation turn a simple bulk import into a strong portfolio demonstration of enterprise-scale thinking.

**Related Files**
* [OU Management Runbook](../ou-management-runbook.md)
* [Engineering User Onboarding](../engineering-user-onboarding.md)
* [GPO Overview](../gpo/gpo-overview.md)
* [AGDLP Design](../../architecture/agdlp-design.md)
```
