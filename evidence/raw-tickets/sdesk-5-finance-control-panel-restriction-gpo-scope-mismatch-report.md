````markdown
# Finance Control Panel Restriction GPO Scope Mismatch

## Ticket Type

Internal IT Task / Configuration Validation

## Category

Group Policy / Endpoint Configuration

## Priority

Medium

## Severity

Low

## Summary

Post-deployment GPO validation identified a scope mismatch affecting one Finance user. The Finance Control Panel restriction GPO was configured and functioning for correctly scoped users, but `TestUserFin` was excluded from the required GPO security filtering group. As a result, the user did not receive the intended Control Panel and Windows Settings restriction policy.

---

# Ticket-Facing Documentation

## Symptoms

Post-deployment audit identified that `TestUserFin` was excluded from the Finance Control Panel restriction GPO scope.

The affected user was located in the correct Finance Users OU but did not receive `GPO_Finance_Restrict_ControlPanel`.

## Scope / Impact

- **Affected user(s):** `TestUserFin`
- **Device(s):** `WindowsADClient`
- **Department:** Finance
- **Business impact:** Low
- **Operational impact:** One Finance user was not receiving the intended endpoint restriction policy.
- **Security/configuration impact:** Inconsistent enforcement of Finance desktop restriction baseline.

## Environment

- **Hostname:** `WindowsADClient`
- **OS:** Windows 10 Pro
- **Domain:** `lab.local`
- **Location/VPN:** Internal domain-connected workstation
- **Related systems/services:**
  - Active Directory
  - Group Policy
  - Finance Users OU
  - GPO security filtering

## Baseline Checks

- Confirmed Finance user policy processing was functional.
- Confirmed `GPO_Finance_Restrict_ControlPanel` was configured.
- Confirmed GPO was linked to the Finance Users OU.
- Confirmed the GPO was security-filtered to `GG_Finance_ControlPanel_Restricted`.
- Confirmed `TestUserFin` was located in the correct Finance Users OU.
- Confirmed `TestUserFin` was missing from the required policy security group.

## Troubleshooting Performed

1. Reviewed Finance user OU membership.
2. Reviewed membership of `GG_Finance_ControlPanel_Restricted`.
3. Compared expected Finance users against actual members of the GPO security filtering group.
4. Identified `TestUserFin` as present in the Finance Users OU but missing from `GG_Finance_ControlPanel_Restricted`.
5. Confirmed GPO scope mismatch caused the affected user to be excluded from policy application.
6. Added `TestUserFin` to `GG_Finance_ControlPanel_Restricted`.
7. Refreshed policy/user context and validated successful policy application.
8. Confirmed Control Panel and Windows Settings restriction applied successfully.

## Findings

`TestUserFin` was located in the correct Finance Users OU but was not a member of `GG_Finance_ControlPanel_Restricted`.

Because `GPO_Finance_Restrict_ControlPanel` was security-filtered to that group, the affected user was excluded from applying the GPO.

## Root Cause

The affected user was missing from the policy-specific GPO security filtering group required to apply the Finance Control Panel restriction policy.

## Resolution

Added `TestUserFin` to `GG_Finance_ControlPanel_Restricted`.

After policy refresh/user context renewal, the affected user successfully received `GPO_Finance_Restrict_ControlPanel`.

## Validation

- Confirmed `TestUserFin` is now a member of `GG_Finance_ControlPanel_Restricted`.
- Confirmed `GPO_Finance_Restrict_ControlPanel` applied successfully.
- Confirmed Control Panel and Windows Settings access was restricted as expected.

## Closure Notes

Post-deployment validation identified that `TestUserFin` was excluded from the Finance Control Panel restriction GPO due to missing membership in `GG_Finance_ControlPanel_Restricted`. The user was added to the required policy security group, policy application was validated, and Control Panel/Settings restrictions were confirmed successfully.

---

# Internal Jira / Lab-Facing Notes

## Scenario Purpose

This ticket was built to simulate a realistic internal IT validation task rather than a normal end-user incident.

The issue does **not** originate from a user reporting that Control Panel is available. That would be unrealistic because most users would not know or care that Control Panel should be blocked.

Instead, the scenario is framed as a post-deployment audit where IT validates whether a department-specific GPO is applying correctly to all intended Finance users.

## Scenario Backstory

A Finance department restriction GPO was deployed to block access to Control Panel and Windows Settings.

The intended policy design was:

```text
Finance Users OU
        ↓
GPO_Finance_Restrict_ControlPanel
        ↓
Security Filtering:
GG_Finance_ControlPanel_Restricted
````

Users who are members of `GG_Finance_ControlPanel_Restricted` should receive the policy and be blocked from accessing Control Panel and Windows Settings.

The broken condition was created by keeping `TestUserFin` in the correct Finance Users OU but intentionally leaving the user out of `GG_Finance_ControlPanel_Restricted`.

This created a realistic GPO scope mismatch.

## GPO Built

```text
GPO Name:
GPO_Finance_Restrict_ControlPanel
```

Policy configured:

```text
User Configuration
→ Policies
→ Administrative Templates
→ Control Panel
→ Prohibit access to Control Panel and PC settings
→ Enabled
```

GPO linked to:

```text
OU=Users,OU=Finance,OU=Accounts,DC=lab,DC=local
```

Security filtering:

```text
GG_Finance_ControlPanel_Restricted
```

Delegation / read permission:

```text
Domain Computers = Read only
```

Important note:

`Domain Computers` was needed for read access after removing `Authenticated Users`, but it should not have `Apply group policy` for this user-side restriction scenario.

## Known-Good Control Validation

Before testing the broken user, the GPO was validated with known-good Finance user:

```text
SladePinel
```

Purpose of this step:

* confirm GPO setting was correct
* confirm Finance OU link was correct
* confirm security filtering worked
* confirm workstation could read the GPO
* confirm the restriction actually blocked Control Panel/Settings

Result:

```text
SladePinel received GPO_Finance_Restrict_ControlPanel.
Control Panel and Windows Settings were blocked successfully.
```

This established that the GPO itself worked before troubleshooting `TestUserFin`.

## Broken-State User

Affected test user:

```text
LAB\TestUserFin
```

Confirmed user location:

```text
CN=Test User Finance,OU=Users,OU=Finance,OU=Accounts,DC=lab,DC=local
```

Initial group membership:

```text
Domain Users
GG_Finance_Users
GG_Champion_Users
```

Missing required group:

```text
GG_Finance_ControlPanel_Restricted
```

Expected broken behavior:

```text
TestUserFin receives normal Finance policies but does not receive the Control Panel restriction GPO.
```

## PowerShell Audit Commands Used

### Pull Finance OU users

```powershell
$financeUsers = Get-ADUser -SearchBase "OU=Users,OU=Finance,OU=Accounts,DC=lab,DC=local" -Filter * |
    Select-Object -ExpandProperty SamAccountName
```

Purpose:

```text
Collect all expected Finance users from the Finance Users OU.
```

### Pull policy group members

```powershell
$policyMembers = Get-ADGroupMember "GG_Finance_ControlPanel_Restricted" -Recursive |
    Where-Object objectClass -eq "user" |
    Select-Object -ExpandProperty SamAccountName
```

Purpose:

```text
Collect all users currently scoped to receive the Control Panel restriction GPO.
```

### Compare expected users vs scoped users

```powershell
Compare-Object $financeUsers $policyMembers
```

Output:

```text
InputObject SideIndicator
----------- -------------
TestUserFin <=
```

Meaning:

```text
TestUserFin exists in the Finance Users OU but does not exist in GG_Finance_ControlPanel_Restricted.
```

This was the core audit finding.

## GPResult Validation Command

```cmd
gpresult /S WindowsADClient /USER LAB\TestUserFin /H C:\Temp\TestUserFin-GPReport.html
```

Purpose:

```text
Generate a Group Policy Results report for the affected user/computer context.
```

This helps validate whether the GPO applied, was denied, or was filtered out.

Important limitation:

The user generally needs an existing logon/session history on the workstation for useful RSOP data to exist.

## Resolution Command

```powershell
Add-ADGroupMember -Identity "GG_Finance_ControlPanel_Restricted" -Members "TestUserFin"
```

Purpose:

```text
Add the affected user to the policy-specific security group required to apply the GPO.
```

## Membership Validation Command

```powershell
Get-ADPrincipalGroupMembership TestUserFin | Select-Object Name
```

Expected result after fix:

```text
GG_Finance_ControlPanel_Restricted
```

should appear in the user’s group membership list.

## Post-Fix Validation

After adding the user to the group, the user context was refreshed and the GPO was validated.

Validation confirmed:

```text
TestUserFin received GPO_Finance_Restrict_ControlPanel.
Control Panel access was blocked.
Windows Settings access was blocked.
```

## Evidence Captured

Screenshots/evidence to keep with this ticket:

1. GPO configuration showing correct Control Panel restriction setting.
2. GPO scope/security filtering showing `GG_Finance_ControlPanel_Restricted`.
3. Delegation showing `Domain Computers` with read access.
4. Known-good control validation showing restriction works for SladePinel.
5. PowerShell output showing `TestUserFin` missing from policy group comparison.
6. Resolution command adding `TestUserFin` to the group.
7. Membership validation showing `TestUserFin` in `GG_Finance_ControlPanel_Restricted`.
8. Final validation showing Control Panel/Settings blocked.

## Key Learning Points

### OU linkage does not equal GPO application

A user can be in the correct OU and still not receive a GPO if security filtering excludes them.

### Security filtering controls who can apply the GPO

For this scenario:

```text
GG_Finance_ControlPanel_Restricted
```

was the group allowed to apply the GPO.

If the user is missing from that group, the GPO does not apply even if the user is in the correct OU.

### Domain Computers read access matters

When `Authenticated Users` is removed from GPO security filtering, computers may still need read access to the GPO. This is why `Domain Computers` was added with read-only permission through Delegation.

### Internal audit tickets are different from user incidents

This ticket should not be framed as:

```text
User reported they can access Control Panel.
```

Better framing:

```text
Post-deployment validation identified a GPO scope mismatch affecting one Finance user.
```

### Ticket-facing notes should stay concise

The ticket should focus on:

* issue
* scope
* root cause
* fix
* validation

### Internal notes should preserve the full backstory

Internal notes should include:

* how the scenario was built
* commands used
* why each step mattered
* screenshots/evidence
* gotchas
* validation logic
* what to remember later

## Professional Summary

This scenario demonstrates a realistic internal IT configuration validation workflow. The issue was discovered through audit-style comparison of expected Finance users against the GPO security filtering group. The root cause was not a broken GPO, broken OU link, or workstation issue. The root cause was a user missing from the policy-specific security group required to apply the Finance Control Panel restriction GPO.

The fix was completed by adding the affected user to the correct group and validating successful GPO application afterward.

```
```
