# Group Policy Object (GPO) Management Runbook

**Last Updated:** May 22, 2026  
**Domain:** lab.local  
**Purpose:** Comprehensive guide to the full GPO lifecycle — pre-flight verification, manual GUI creation, automated scripted deployment, verification, and troubleshooting — for departmental policy enforcement in a mid-sized enterprise lab environment.

## Executive Summary / Business Rationale

Group Policy is the primary mechanism for enforcing security baselines, desktop standardization, drive mappings, folder redirection, and departmental restrictions across the lab. This runbook documents the complete process used to roll out GPOs for the Engineering and Sales departments, demonstrating both manual and automated approaches at scale (~1,000 users).

The project followed a deliberate two-phase approach:
- **Phase 1 (Manual GUI)**: Sales department GPO created via GPMC to validate settings and workflow.
- **Phase 2 (Scripted)**: Engineering department GPO deployed via PowerShell for repeatability and speed.

All changes were preceded by rigorous pre-flight checks and Azure restore points to ensure zero-risk deployment.

---

## Pre-flight Checks (Mandatory Safety Net)

### Design Decision – Why Pre-flight Checks Were Mandatory

The decision to perform a comprehensive pre-flight verification before any GPO changes was deliberate and rooted in risk mitigation. The goal was to **prevent as many issues during the rollout as possible**, ensuring the rollout was not hampered by troubleshooting, lab-breaking screwups, or even minor annoyances.

### Detailed Pre-flight Verification Steps

1. **Server Manager Access Verification**  
   * *See Evidence:* [01-server-manager-screen.png](../../screenshots/group-object-policy-management/01-server-manager-screen.png)

2. **Active Directory Users and Computers (ADUC) Navigation**  
   * *See Evidence:* [02-navigate-to-servers-aduc.png](../../screenshots/group-object-policy-management/02-navigate-to-servers-aduc.png)

3. **Domain Admin Privilege Confirmation (“God Mode” Check)**  
   * *See Evidence:* [03-domain-admin-god-mode-check.png](../../screenshots/group-object-policy-management/03-domain-admin-god-mode-check.png)

4. **Client-Side Asset Validation**  
   * *See Evidence:* [04-client-shared-folders-wallpaper-check.png](../../screenshots/group-object-policy-management/04-client-shared-folders-wallpaper-check.png)

5. **Azure VM Restore Point Creation**  
   * *See Evidence:* [05-azure-restore-point-navigation.png](../../screenshots/group-object-policy-management/05-azure-restore-point-navigation.png)  
   * *See Evidence:* [06-restore-point-name-and-date.png](../../screenshots/group-object-policy-management/06-restore-point-name-and-date.png)  
   * *See Evidence:* [07-restore-point-review-and-create.png](../../screenshots/group-object-policy-management/07-restore-point-review-and-create.png)  
   * *See Evidence:* [08-restore-point-click-create.png](../../screenshots/group-object-policy-management/08-restore-point-click-create.png)  
   * *See Evidence:* [09-restore-point-verify-successful-deployment.png](../../screenshots/gpo-management/09-restore-point-verify-successful-deployment.png)
6. **Final Script & Environment Readiness Check**  
   * *See Evidence:* [10-pre-flight-script-execution-start.png](../../screenshots/group-object-policy-management/10-pre-flight-script-execution-start.png)

---

## Manual GUI Method – Sales Department Example (Phase 1)

1. In Group Policy Management, navigate to the Sales OU.  
   * *See Evidence:* [01-in-aduc-select-ou.png](../../screenshots/group-object-policy-management/01-in-aduc-select-ou.png)

2. Right-click the OU → **Create a GPO in this domain, and Link it here**.  
   * *See Evidence:* [02-create-gpo-and-link-here.png](../../screenshots/group-object-policy-management/02-create-gpo-and-link-here.png)

3. Name the GPO (`Sales-Users`) and click **OK**.  
   * *See Evidence:* [03-name-gpo-and-ok.png](../../screenshots/group-object-policy-management/03-name-gpo-and-ok.png)

4. Right-click the new GPO → **Edit**.  
   * *See Evidence:* [04-right-click-gpo-edit.png](../../screenshots/group-object-policy-management/04-right-click-gpo-edit.png)

5. Expand **User Configuration** → **Preferences**.  
   * *See Evidence:* [05a-expand-user-config-preferences.png](../../screenshots/group-object-policy-management/05a-expand-user-config-preferences.png)

6. Navigate to **Drive Maps** → **New** → **Mapped Drive**.  
   * *See Evidence:* [06-click-drive-maps.png](../../screenshots/group-object-policy-management/06-click-drive-maps.png) and [07-new-mapped-drive.png](../../screenshots/group-object-policy-management/07-new-mapped-drive.png)

7. Configure drive properties.  
   * *See Evidence:* [08a-mapped-drive-properties.png](../../screenshots/group-object-policy-management/08a-mapped-drive-properties.png) and [08b-mapped-drive-properties.png](../../screenshots/group-object-policy-management/08b-mapped-drive-properties.png)

8. Enable Item-Level Targeting.  
   * *See Evidence:* [09a-item-level-targeting.png](../../screenshots/group-object-policy-management/09a-item-level-targeting.png) and [09b-item-level-targeting.png](../../screenshots/group-object-policy-management/09b-item-level-targeting.png)

9. Configure Desktop Wallpaper policy.  
   * *See Evidence:* [10a-policies-desktop-folder.png](../../screenshots/group-object-policy-management/10a-policies-desktop-folder.png) and [11a-desktop-wallpaper-settings.png](../../screenshots/group-object-policy-management/11a-desktop-wallpaper-settings.png)

10. Set full UNC path and Fill style, then apply.  
    * *See Evidence:* [12a-wallpaper-path-and-style.png](../../screenshots/group-object-policy-management/12a-wallpaper-path-and-style.png) and [13a-wallpaper-apply-ok.png](../../screenshots/group-object-policy-management/13a-wallpaper-apply-ok.png)

---

## Automated Scripted Method – Engineering Department Rollout (Phase 2)

*See Evidence:*  
- [01-eng-dept-gpo-rollout-success.png](../../screenshots/group-object-policy-management/01-eng-dept-gpo-rollout-success.png)  
- [02-eng-dept-gpo-rollout-success.png](../../screenshots/group-object-policy-management/02-eng-dept-gpo-rollout-success.png)  
- [03-eng-dept-gpo-rollout-success.png](../../screenshots/group-object-policy-management/03-eng-dept-gpo-rollout-success.png)  
- [04-eng-dept-gpo-rollout-success.png](../../screenshots/group-object-policy-management/04-eng-dept-gpo-rollout-success.png)  
- [05-eng-dept-gpo-rollout-success.png](../../screenshots/group-object-policy-management/05-eng-dept-gpo-rollout-success.png)  
- [06-eng-task-bar-gpo-script-output.png](../../screenshots/group-object-policy-management/06-eng-task-bar-gpo-script-output.png)

---

## Verification

```powershell
Get-GPO -Name "Engineering-Users" | Select DisplayName, GpoStatus, CreationTime, ModificationTime
gpresult /h gpo-report.html /scope user
gpresult /h gpo-report.html /scope computer
```

---

## Troubleshooting (Real Issues Encountered)

- Authenticated Users permission warning → resolved with `Set-GPPermissions`
- Syntax errors in registry value commands → corrected with exact key paths
- GPO not applying immediately → resolved with `gpupdate /force` + logoff/logon

---

## Lessons Learned

- Pre-flight checks and Azure restore points are non-negotiable for controlled changes.
- Scripted GPO deployment is dramatically faster and more repeatable than GUI.
- Item-Level Targeting and Preferences provide powerful user-specific flexibility.

**Related Files**
- [OU Management Runbook](../ou-management-runbook.md)
- [Group Management Runbook](../group-management-runbook.md)
- [Bulk User Import Runbook](../bulk-user-import.md)
```

Save this file, commit, and push.

Let me know if it's good or if you need the next file updated. We're close to finishing the cleanup.