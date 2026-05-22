# Runbook: Join Second Client Workstation (WADC2) to lab.local Domain

**Last Updated:** May 20, 2026  
**Status:** ✅ Completed  
**Jira Reference:** KAN-xxx (Add Second Workstation - WADC2)  

## Executive Summary / Business Rationale

The decision to provision a second client workstation (`WADC2`) was strategic and served **three key purposes**:
1. **Recovery of Lost Documentation** – The original join process and troubleshooting for the first client (`WindowsADClient`) had incomplete or missing screenshots and notes.
2. **Enhanced Testing Capability with Minimized Blast Radius** – Provide an additional isolated workstation for testing complex AD scenarios (delegation, GPO application, user-move operations, etc.).
3. **Redundancy & Resilience** – Ensure the lab remains operational if the primary client becomes unavailable or is intentionally broken during break/fix exercises.

## Lab Context

| Item                  | Value                                      |
|-----------------------|--------------------------------------------|
| Domain                | lab.local                                  |
| Domain Controller     | WindowsServer1 (10.0.0.5)                  |
| Client 1 (Primary)    | WindowsADClient                            |
| Client 2 (New)        | WADC2 (Windows 10 Pro)                     |
| Target OU             | OU=Workstations,DC=lab,DC=local            |
| Administrator         | lab\David (Domain Admin)                   |

## Prerequisites
- Azure VM in the same VNet/subnet as the Domain Controller
- Local Administrator access on WADC2
- Domain Admin credentials (`lab\David`)
- Azure VNet configured with **Custom DNS** pointing to the DC IP (`10.0.0.5`)

## Step-by-Step Procedure

### 1. Rename Hostname (Pre-Join Best Practice)
- Settings → System → About → Rename this PC → `WADC2`
- Restart if prompted

### 2. Configure Azure VNet DNS (Critical Azure-Specific Step)
1. Azure Portal → Virtual networks → [Your Lab VNet] → DNS servers
2. Change from **Default (Azure-provided)** → **Custom**
3. Add DC private IP: `10.0.0.5`
4. Click **Save**

### 3. Renew IP and Flush DNS on Client
```powershell
# Run as Administrator on WADC2
ipconfig /release
ipconfig /renew
ipconfig /flushdns
```

**Break/Fix Incident (Documented):**  
**Issue:** RDP connection dropped immediately after `/release`  
**Root Cause:** Azure DHCP lease released  
**Resolution:** Restart VM from Azure Portal  
**Lesson Learned:** In Azure, always use the Azure Portal restart after major network changes.

### 4. Verify DNS Resolution
```powershell
nslookup lab.local
nltest /dsgetdc:lab.local
```

**Reference:**  
[Screenshot: ipconfig /all before join](../../screenshots/domain-join/01_ipconfig_all_before_join.png)  
[Screenshot: Network adapter properties (DNS verification)](../../screenshots/domain-join/02_this_pc_properties.png)

### 5. Join Domain (GUI Method)
1. Right-click **This PC** → **Properties** → **Change settings**
2. Click **Change** → Select **Domain** → type `lab.local`
3. Click **OK**
4. Enter credentials: `lab\David` + password
5. You will see “Welcome to the lab.local domain.”
6. Click **OK** on the restart prompt

**Reference:**  
[Screenshot: System Properties – Change to Domain](../../screenshots/domain-join/03_domain_join_window.png)  
[Screenshot: Click OK to join](../../screenshots/domain-join/04_click_ok_to_join.png)  
[Screenshot: Domain credentials prompt](../../screenshots/domain-join/05_domain_credentials_prompt.png)  
[Screenshot: Welcome to the lab.local domain](../../screenshots/domain-join/06_welcome_to_domain_message.png)  
[Screenshot: Restart prompt after join](../../screenshots/domain-join/07_restart_prompt_after_join.png)

### 6. Post-Join Tasks (Run on Domain Controller or RSAT machine)
```powershell
Import-Module ActiveDirectory
Get-ADComputer "WADC2" | Move-ADObject -TargetPath "OU=Workstations,DC=lab,DC=local"
```

## Verification Commands (Run on WADC2 after restart)
```powershell
whoami                          # → lab\david
nltest /dsgetdc:lab.local
Get-ADComputer WADC2 -Properties * | Select Name, DistinguishedName, OperatingSystem
```

**Reference:**  
[Screenshot: whoami after restart](../../screenshots/domain-join/08_whoami_after_restart.png)  
[Screenshot: nltest /dsgetdc success](../../screenshots/domain-join/09_nltest_dsgetdc_success.png)

## Troubleshooting / Break-Fix Section

| Issue                          | Symptom                     | Root Cause                     | Fix                              |
|--------------------------------|-----------------------------|--------------------------------|----------------------------------|
| RDP lost after ipconfig /release | Connection dropped         | Azure DHCP lease released      | Restart VM from Azure Portal     |
| "Cannot find domain"           | nslookup fails              | VNet DNS still default         | Set VNet DNS to DC IP            |
| Join fails with Access Denied  | Credentials rejected        | Wrong username format          | Use `lab\David`                  |
| Computer object in wrong OU    | Appears in default Computers| Not moved after join           | Use Move-ADObject                |

## Lessons Learned
- Always configure VNet DNS **before** client join in Azure environments.
- Hostname change before domain join prevents duplicate computer accounts.
- Multiple clients enable safer, parallel testing and true redundancy.
- Every break (even small ones) becomes valuable documentation.

**Related Files**  
*(To be finalized in the final sweep)*
```

---
