# AD DS Break/Fix: Finance User Unable to Authenticate on New Workstation — Incorrect Username Format (SDESK-2)

## Lab Context

| Item              | Value                                      |
|-------------------|--------------------------------------------|
| Domain            | lab.local                                  |
| Ticket            | SDESK-2                                    |
| Affected User     | Test User Finance (TestUserFin)            |
| Department        | Finance                                    |
| Client            | WindowsADClient (new/reimaged workstation) |
| Issue Type        | Logon name format                          |

## Problem Summary
Finance user could not sign in to a newly provisioned workstation.  
Credentials were correct, but authentication failed with **“The user name or password is incorrect.”**

## Troubleshooting Steps
1. Reproduced failed sign-in using short username `TestUserFin`.
2. Reviewed account details in ADUC.
3. Identified that the workstation required the **domain-qualified** format.
4. Retried sign-in using `LAB\TestUserFin` with the same password.
5. Validated successful desktop access.

## Root Cause
User entered a non-domain-qualified username (`TestUserFin`) on a workstation that expected the down-level format (`LAB\TestUserFin`).

## Resolution
- Corrected sign-in format from `TestUserFin` → `LAB\TestUserFin`.
- User authenticated successfully with the existing password (no reset needed).

## Validation
- User confirmed successful login.
- Full desktop session loaded under the intended Finance user account.

## Full Visual Evidence
**[View complete Folge screenshot bundle](docs/troubleshooting/evidence/SDESK-2-Finance%20user%20unabe%20to%20authenticate%20on%20New%20Workstation%20-%2012-05-2026.pdf)**  
*(3 screenshots: login error → ADUC logon name check → successful desktop)*

## Lessons Learned
- Always test both UPN and `DOMAIN\username` format on new workstations.
- This is a very common silent authentication issue after bulk user creation.

**Reference:** SDESK-2 (Folge export)  

**Related Files:**  
- `docs/troubleshooting/ad-password-reset-sdesk-1.md`

**Status:** Resolved 12-May-2026