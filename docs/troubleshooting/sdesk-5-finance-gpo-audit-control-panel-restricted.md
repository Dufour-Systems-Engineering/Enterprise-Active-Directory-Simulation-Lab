# Finance Control Panel Restriction GPO Scope Mismatch (SDESK-5)

**Last Updated:** May 14, 2026  
**Purpose:** Document the identification and resolution of a GPO security filtering scope mismatch affecting a Finance department user.

## Lab Context

| Item              | Value                                      |
|-------------------|--------------------------------------------|
| Domain            | lab.local                                  |
| Ticket            | SDESK-5                                    |
| Affected User     | TestUserFin                                |
| Department        | Finance                                    |
| Client            | WindowsADClient                            |
| Issue Type        | GPO Security Filtering / Scope Mismatch    |

## Problem Summary
Post-deployment validation revealed that `TestUserFin` was not receiving the `GPO_Finance_Restrict_ControlPanel` policy, even though the user was located in the correct Finance Users OU. Control Panel and Windows Settings restrictions were not being applied.

## Resolution Steps

1. Performed PowerShell audit comparing Finance OU users against the GPO security filtering group.
2. Identified `TestUserFin` was present in `OU=Users,OU=Finance,OU=Accounts,DC=lab,DC=local` but missing from `GG_Finance_ControlPanel_Restricted`.
3. Added `TestUserFin` to the `GG_Finance_ControlPanel_Restricted` group.
4. Refreshed user policy context.
5. Validated successful GPO application.

## Root Cause
The user was missing from the policy-specific security filtering group (`GG_Finance_ControlPanel_Restricted`) required to apply the Finance Control Panel restriction GPO.

## Validation
- Confirmed `TestUserFin` is now a member of `GG_Finance_ControlPanel_Restricted`.
- Verified `GPO_Finance_Restrict_ControlPanel` applied successfully.
- Confirmed Control Panel and Windows Settings were restricted as intended.

## Visual Evidence
**Full walkthrough available in:**  
`screenshots/sdesk-5-finance-control-panel-restriction-gpo-scope-mismatch-report/`

**Key Screenshots:**
- [01-gpo-application-gap-ps-output.png](../../screenshots/sdesk-5-finance-control-panel-restriction-gpo-scope-mismatch-report/01-gpo-application-gap-ps-output.png) — PowerShell output showing GPO scope mismatch
- [02-verification-user-added-to-cp-restricted-gpo.png](../../screenshots/sdesk-5-finance-control-panel-restriction-gpo-scope-mismatch-report/02-verification-user-added-to-cp-restricted-gpo.png) — Verification after adding user to the security group

## Lessons Learned
- OU linkage alone does not guarantee GPO application — security filtering is the final authority.
- Post-deployment audits comparing OU membership vs. GPO security filtering groups are essential for consistency.
- Use dedicated security groups for GPO filtering (especially for department-specific restrictions) to maintain clean AGDLP design.

**Related Files:**
- [Group Management Runbook](../runbooks/group-management-runbook.md)
- [GPO Overview](../gpo/gpo-overview.md)

**Status:** Resolved