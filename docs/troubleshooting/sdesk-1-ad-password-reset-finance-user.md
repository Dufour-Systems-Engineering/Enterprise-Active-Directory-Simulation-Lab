# AD Password Reset - Finance Test User (SDESK-1)

**Last Updated:** May 12, 2026  
**Purpose:** Document the resolution of a standard Tier 1 password reset request for a Finance department test user.

## Lab Context

| Item            | Value                          |
|-----------------|--------------------------------|
| Domain          | lab.local                      |
| Ticket          | SDESK-1                        |
| Affected User   | TestUserFin                    |
| Department      | Finance                        |
| Client          | WindowsADClient                |
| Issue Type      | Password Reset                 |

## Problem Summary
A Finance test user (`TestUserFin`) could not log into the domain workstation. The login attempt returned:  
**"The user name or password is incorrect. Try again."**

## Resolution Steps

1. Reproduced the login failure on `WindowsADClient`.
2. Located the user account in **Active Directory Users and Computers**.
3. Right-clicked the user → **Reset Password**, set a temporary password, and enabled **"User must change password at next logon"**.
4. Confirmed the password reset completed successfully.
5. Logged in with the temporary password and completed the forced change.
6. Verified full access to the desktop and Finance department resources.

## Root Cause
Standard forgotten/expired password scenario. No account lockout, replication issues, or GPO conflicts were present.

## Validation
- Successful login with new password.
- Desktop loaded with correct Finance department GPO settings (wallpaper, drive mappings).
- No residual access or policy issues observed.

## Visual Evidence
**Full walkthrough available in:**  
`screenshots/sdesk-1-ad-password-reset-finance-user/`

**Key Screenshots:**
- [01-login-failure-screen.png](../../screenshots/sdesk-1-ad-password-reset-finance-user/01-login-failure-screen.png) — Initial login error
- [02-password-reset-dialog-in-aduc.png](../../screenshots/sdesk-1-ad-password-reset-finance-user/02-password-reset-dialog-in-aduc.png) — Password reset dialog in ADUC
- [03-confirmation-of-reset.png](../../screenshots/sdesk-1-ad-password-reset-finance-user/03-confirmation-of-reset.png) — Confirmation of password change
- [04-successful-desktop-login-after-password-change.png](../../screenshots/sdesk-1-ad-password-reset-finance-user/04-successful-desktop-login-after-password-change.png) — Successful desktop after reset

## Lessons Learned
- Always enable **"User must change password at next logon"** for security and best practice.
- This is a foundational Tier 1 Help Desk task and serves as good baseline validation for delegation testing.

**Related Files:**
- [Finance User Workstation Authentication Failure (SDESK-2)](../troubleshooting/sdesk-2-finance-user-workstation-authentication-failure.md)
- [Tier 1 Help Desk Delegation Runbook](../runbooks/tier-1-support-delegation.md)

**Status:** Resolved
```