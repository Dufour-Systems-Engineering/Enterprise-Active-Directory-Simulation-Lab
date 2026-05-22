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

The decision to perform a comprehensive pre-flight verification before any GPO changes was deliberate and rooted in risk mitigation. The goal was to **prevent as many issues during the rollout as possible**, ensuring the rollout was not hampered by troubleshooting, lab-breaking screwups, or even minor annoyances that could derail momentum or introduce unnecessary complexity. 

In a real enterprise environment, a failed or partially successful GPO rollout can cause widespread user impact, wasted administrative time, and loss of confidence in the change process. By front-loading verification, we eliminated common failure points (missing permissions, missing files, incorrect domain admin context, unverified assets, and lack of rollback capability) before they could become problems. This approach mirrors production best practices and turned what could have been a high-risk operation into a controlled, repeatable, and low-stress exercise.

### Detailed Pre-flight Verification Steps

1. **Server Manager Access Verification** – Confirmed that the full administrative Tools menu (including Group Policy Management) was available and functional.  
   * *See Evidence:* [01-server-manager-screen.png](../../screenshots/group-object-policy-management/01-server-manager-screen.png)
2. **Active Directory Users and Computers (ADUC) Navigation** – Verified full read/write access to the domain structure and departmental OUs.  
   * *See Evidence:* [02-navigate-to-servers-aduc.png](../../screenshots/group-object-policy-management/02-navigate-to-servers-aduc.png)
3. **Domain Admin Privilege Confirmation (“God Mode” Check)** – Explicitly verified that the administrative account had full Domain Admin rights and was not running under a limited context.  
   * *See Evidence:* [03-domain-admin-god-mode-check.png](../../screenshots/group-object-policy-management/03-domain-admin-god-mode-check.png)
4. **Client-Side Asset Validation** – Confirmed all required departmental wallpaper images and shared folder structures already existed and were accessible from the client workstation.  
   * *See Evidence:* [04-client-shared-folders-wallpaper-check.png](../../screenshots/group-object-policy-management/04-client-shared-folders-wallpaper-check.png)
5. **Azure VM Restore Point Creation** – Created a named, time-stamped restore point on `WindowsServer1` immediately before any GPO modifications.  
   * *See Evidence Pipeline:* [05-azure-restore-point-navigation.png](../../screenshots/group-object-policy-management/05-azure-restore-point-navigation.png), [06-restore-point-name-and-date.png](../../screenshots/group-object-policy-management/06-restore-point-name-and-date.png), [07-restore-point-review-and-create.png](../../screenshots/group-object-policy-management/07-restore-point-review-and-create.png), [08-restore-point-click-create.png](../../screenshots/group-object-policy-management/08-restore-point-click-create.png), and [09-restore-point-deployment-complete.png](../../screenshots/group-object-policy-management/09-restore-point-deployment-complete.png)
6. **Final Script & Environment Readiness Check** – Opened an elevated PowerShell session, confirmed all GPO scripts were present, and validated the execution environment.  
   * *See Evidence:* [10-pre-flight-script-execution-start.png](../../screenshots/group-object-policy-management/10-pre-flight-script-execution-start.png)

---

## Manual GUI Method – Sales Department Example (Phase 1)

1. In Group Policy Management, navigate to the target Sales organizational unit container.  
   * *See Evidence:* [01-in-aduc-select-ou.png](../../screenshots/group-object-policy-management/01-in-aduc-select-ou.png)
2. Right-click the OU → **Create a GPO in this domain, and Link it here**.  
   * *See Evidence:* [02-create-gpo-and-link-here.png](../../screenshots/group-object-policy-management/02-create-gpo-and-link-here.png)
3. Name the GPO (`Sales-Users`) and finalize creation context.  
   * *See Evidence:* [03-name-gpo-and-ok.png](../../screenshots/group-object-policy-management/03-name-gpo-and-ok.png)
4. Right-click the newly provisioned GPO object → **Edit** to open the management console workspace.  
   * *See Evidence:* [04-right-click-gpo-edit.png](../../screenshots/group-object-policy-management/04-right-click-gpo-edit.png)
5. Expand **User Configuration** → **Preferences** to navigate the setting hierarchy.  
   * *See Evidence:* [05a-expand-user-config-preferences.png](../../screenshots/group-object-policy-management/05a-expand-user-config-preferences.png)
6. Navigate to **Drive Maps** → **New** → **Mapped Drive**.  
   * *See Evidence:* [06-click-drive-maps.png](../../screenshots/group-object-policy-management/06-click-drive-maps.png) and [07-new-mapped-drive.png](../../screenshots/group-object-policy-management/07-new-mapped-drive.png)
7. Configure core drive properties, setting the action matrix to Update and assigning target label strings.  
   * *See Evidence:* [08a-mapped-drive-properties.png](../../screenshots/group-object-policy-management/08a-mapped-drive-properties.png) and [08b-mapped-drive-properties.png](../../screenshots/group-object-policy-management/08b-mapped-drive-properties.png)
8. Enable Advanced Item-Level Targeting to enforce exact security filtering parameters.  
   * *See Evidence:* [09a-item-level-targeting.png](../../screenshots/group-object-policy-management/09a-item-level-targeting.png) and [09b-item-level-targeting.png](../../screenshots/group-object-policy-management/09b-item-level-targeting.png)
9. Navigate to the Desktop Policy node to build corporate personalization baselines.  
   * *See Evidence:* [10a-policies-desktop-folder.png](../../screenshots/group-object-policy-management/10a-policies-desktop-folder.png) and [11a-desktop-wallpaper-settings.png](../../screenshots/group-object-policy-management/11a-desktop-wallpaper-settings.png)
10. Input full UNC target share paths, specify the background Fill style parameter, and commit properties.  
    * *See Evidence:* [12a-wallpaper-path-and-style.png](../../screenshots/group-object-policy-management/12a-wallpaper-path-and-style.png) and [13a-wallpaper-apply-ok.png](../../screenshots/group-object-policy-management/13a-wallpaper-apply-ok.png)

---

## Automated Scripted Method – Engineering Department Rollout (Phase 2)

The Engineering GPO was fully created and configured via PowerShell for speed and repeatability. The script handled security permission fixes, registry settings (wallpaper, homepage, drive mapping, folder redirection), taskbar layout, and execution policy enforcement.

### Automated Run Logs
* **Security Infrastructure Baseline Configuration:** [01-eng-dept-gpo-rollout-success.png](../../screenshots/group-object-policy-management/01-eng-dept-gpo-rollout-success.png)
* **Security Group Security Descriptor Assignment:** [02-eng-dept-gpo-rollout-success.png](../../screenshots/group-object-policy-management/02-eng-dept-gpo-rollout-success.png)
* **User Preferences Application Configuration:** [03-eng-dept-gpo-rollout-success.png](../../screenshots/group-object-policy-management/03-eng-dept-gpo-rollout-success.png)
* **Folder Redirection Ingestion Routines:** [04-eng-dept-gpo-rollout-success.png](../../screenshots/group-object-policy-management/04-eng-dept-gpo-rollout-success.png)
* **Computer Subsystem Desired State Configuration:** [05-eng-dept-gpo-rollout-success.png](../../screenshots/group-object-policy-management/05-eng-dept-gpo-rollout-success.png)
* **Taskbar XML Manifest Generation & GPO Linking:** [06-eng-task-bar-gpo-script-output.png](../../screenshots/group-object-policy-management/06-eng-task-bar-gpo-script-output.png)

---

## Verification

To pull current policy metadata and verify live group configurations across both server engines and target clients, run the following commands:

```powershell
Get-GPO -Name "Engineering-Users" | Select DisplayName, GpoStatus, CreationTime, ModificationTime
gpresult /h gpo-report.html /scope user
gpresult /h gpo-report.html /scope computer

```

---

## Troubleshooting (Real Issues Encountered)

* **Authenticated Users Security Warning** → Resolved systematically by executing the `Set-GPPermissions` cmdlet to preserve domain-wide read rights while filtering execution targets.
* **Syntax Exceptions in Registry Commands** → Corrected via explicitly mapping fully-qualified hive paths and validating string/DWORD key parameters.
* **Replication/Caching Processing Delays** → Resolved via forcing background execution checks using `gpupdate /force` combined with complete target session logoffs.

---

## Lessons Learned

* Pre-flight checks and Azure restore points are non-negotiable for controlled, low-risk changes.
* Scripted GPO deployment is dramatically faster and more repeatable than GUI.
* Item-Level Targeting and Preferences provide powerful user-specific flexibility.
* Documenting both manual and automated methods demonstrates deep understanding of GPO management at enterprise scale.

**Related Files**

* [Bulk User Import Runbook](https://www.google.com/search?q=bulk-user-import.md)
* [OU Management Runbook](https://www.google.com/search?q=ou-management-runbook.md)
* [Group Management Runbook](https://www.google.com/search?q=group-management-runbook.md)
* [GPO Overview](https://www.google.com/search?q=../gpo/gpo-overview.md)

```

```