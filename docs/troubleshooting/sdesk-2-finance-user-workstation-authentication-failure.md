# Finance User Workstation Authentication Failure (SDESK-2)

**Last Updated:** May 12, 2026  
**Purpose:** Document the resolution of a common username format authentication failure on a newly provisioned workstation.

## Lab Context

| Item              | Value                                      |
|-------------------|--------------------------------------------|
| Domain            | lab.local                                  |
| Ticket            | SDESK-2                                    |
| Affected User     | TestUserFin                                |
| Department        | Finance                                    |
| Client            | WindowsADClient (new/reimaged)             |
| Issue Type        | Logon Name Format                          |

## Problem Summary
A Finance test user (`TestUserFin`) could not sign in to a newly provisioned workstation. The error displayed was:  
**"The user name or password is incorrect. Try again."**

## Resolution Steps

1. Reproduced the failed sign-in attempt using the short username `TestUserFin`.
2. Reviewed the user account properties in **Active Directory Users and Computers**.
3. Identified that the logon name format was the issue (workstation expected domain-qualified format).
4. Retried login using the down-level format `LAB\TestUserFin`.
5. Confirmed successful authentication and desktop access.

## Root Cause
The user attempted to log in with the short username (`TestUserFin`) instead of the required down-level domain format (`LAB\TestUserFin`) on a new workstation.

## Validation
- Successful login using correct format `LAB\TestUserFin`.
- Full desktop session loaded with Finance department GPO settings applied.
- No password reset or account issues were required.

## Visual Evidence
**Full walkthrough available in:**  
`screenshots/sdesk-2-finance-user-workstation-authentication-failure/`

**Key Screenshots:**
- [01-login-failure.png](../../screenshots/sdesk-2-finance-user-workstation-authentication-failure/01-login-failure.png) — Initial login error
- [02-incorrect-logon-name.png](../../screenshots/sdesk-2-finance-user-workstation-authentication-failure/02-incorrect-logon-name.png) — User Properties showing logon name
- [03-successful-login.png](../../screenshots/sdesk-2-finance-user-workstation-authentication-failure/03-successful-login.png) — Successful desktop after using `LAB\TestUserFin`

## Lessons Learned
- Always test both UPN (`user@domain.local`) and down-level (`DOMAIN\username`) formats when troubleshooting logon issues on new workstations.
- This is a very common issue after bulk user creation or workstation reimaging.

**Related Files:**
- [AD Password Reset - Finance Test User (SDESK-1)](../troubleshooting/sdesk-1-ad-password-reset-finance-user.md)
- [Tier 1 Help Desk Delegation Runbook](../runbooks/tier-1-support-delegation.md)

**Status:** Resolved
```
