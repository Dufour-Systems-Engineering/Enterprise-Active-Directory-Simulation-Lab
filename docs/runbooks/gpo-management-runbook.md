**✅ GPO Management Runbook – All Screenshot Links Fixed**

Copy the entire block below and replace the content of **`docs/runbooks/gpo-management-runbook.md`**:

```markdown
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

1. **Server Manager Access Verification**  
   * *See Evidence:* [01-server-manager-screen.png](../../screenshots/gpo-management/01-server-manager-screen.png)

2. **Active Directory Users and Computers (ADUC) Navigation**  
   * *See Evidence:* [02-navigate-to-servers-aduc.png](../../screenshots/gpo-management/02-navigate-to-servers-aduc.png)

3. **Domain Admin Privilege Confirmation**  
   * *See Evidence:* [03-domain-admin-god-mode-check.png](../../screenshots/gpo-management/03-domain-admin-god-mode-check.png)

4. **Client-Side Asset Validation**  
   * *See Evidence:* [04-client-shared-folders-wallpaper-check.png](../../screenshots/gpo-management/04-client-shared-folders-wallpaper-check.png)

5. **Azure VM Restore Point Creation**  
   * *See Evidence:* [05-azure-restore-point-navigation.png](../../screenshots/gpo-management/05-azure-restore-point-navigation.png)  
   * *See Evidence:* [06-restore-point-name-and-date.png](../../screenshots/gpo-management/06-restore-point-name-and-date.png)  
   * *See Evidence:* [07-restore-point-review-and-create.png](../../screenshots/gpo-management/07-restore-point-review-and-create.png)  
   * *See Evidence:* [08-restore-point-click-create.png](../../screenshots/gpo-management/08-restore-point-click-create.png)  
   * *See Evidence:* [09-restore-point-verify-successful-deployment.png](../../screenshots/gpo-management/09-restore-point-verify-successful-deployment.png)

6. **Final Script & Environment Readiness Check**  
   * *See Evidence:* [10-pre-flight-script-execution-start.png](../../screenshots/gpo-management/10-pre-flight-script-execution-start.png)

---

## Manual GUI Method – Sales Department Example (Phase 1)

1. In Group Policy Management, navigate to the Sales OU.  
   * *See Evidence:* [01-in-aduc-select-ou.png](../../screenshots/gpo-management/01-in-aduc-select-ou.png)

2. Right-click the OU → **Create a GPO in this domain, and Link it here**.  
   * *See Evidence:* [02-create-gpo-and-link-here.png](../../screenshots/gpo-management/02-create-gpo-and-link-here.png)

3. Name the GPO (`Sales-Users`) and click **OK**.  
   * *See Evidence:* [03-name-gpo-and-ok.png](../../screenshots/gpo-management/03-name-gpo-and-ok.png)

4. Right-click the new GPO → **Edit**.  
   * *See Evidence:* [04-right-click-gpo-edit.png](../../screenshots/gpo-management/04-right-click-gpo-edit.png)

5. Expand **User Configuration** → **Preferences**.  
   * *See Evidence:* [05a-expand-user-config-preferences.png](../../screenshots/gpo-management/05a-expand-user-config-preferences.png)

6. Navigate to **Drive Maps**.  
   * *See Evidence:* [06-click-drive-maps.png](../../screenshots/gpo-management/06-click-drive-maps.png)

7. Create new Mapped Drive.  
   * *See Evidence:* [07-new-mapped-drive.png](../../screenshots/gpo-management/07-new-mapped-drive.png)

8. Configure Mapped Drive properties.  
   * *See Evidence:* [08a-mapped-drive-properties.png](../../screenshots/gpo-management/08a-mapped-drive-properties.png)

9. Enable Item-Level Targeting.  
   * *See Evidence:* [09a-item-level-targeting.png](../../screenshots/gpo-management/09a-item-level-targeting.png)

10. Configure Desktop Wallpaper.  
    * *See Evidence:* [11a-desktop-wallpaper-settings.png](../../screenshots/gpo-management/11a-desktop-wallpaper-settings.png)

11. Set wallpaper path and style.  
    * *See Evidence:* [12a-wallpaper-path-and-style.png](../../screenshots/gpo-management/12a-wallpaper-path-and-style.png)

---

## Automated Scripted Method – Engineering Department Rollout (Phase 2)

*See Evidence:*  
- [01-eng-dept-gpo-rollout-success.png](../../screenshots/gpo-management/01-eng-dept-gpo-rollout-success.png)  
- [02-eng-dept-gpo-rollout-success.png](../../screenshots/gpo-management/02-eng-dept-gpo-rollout-success.png)  
- [03-eng-dept-gpo-rollout-success.png](../../screenshots/gpo-management/03-eng-dept-gpo-rollout-success.png)  
- [04-eng-dept-gpo-rollout-success.png](../../screenshots/gpo-management/04-eng-dept-gpo-rollout-success.png)  
- [05-eng-dept-gpo-rollout-success.png](../../screenshots/gpo-management/05-eng-dept-gpo-rollout-success.png)  
- [06-eng-task-bar-gpo-script-output.png](../../screenshots/gpo-management/06-eng-task-bar-gpo-script-output.png)

---

## Verification

```powershell
Get-GPO -Name "Engineering-Users" | Select DisplayName, GpoStatus, CreationTime, ModificationTime
gpresult /h gpo-report.html /scope user
```

---

## Lessons Learned

- Pre-flight checks and Azure restore points are non-negotiable.
- Scripted GPO deployment is dramatically faster than GUI.
- Documenting both manual and automated methods shows depth.

**Related Files**
- [OU Management Runbook](../ou-management-runbook.md)
- [Group Management Runbook](../group-management-runbook.md)
```
