Description

## Symptoms

 Finance user was unable to sign in to the domain workstation using their current known password.

Login attempt returned an incorrect username/password authentication error.

## Scope / Impact

Affected user(s): Finance Test User

Device(s):Windows AD Client

Department: Finance

Business impact: User could not access the workstation or begin work until credentials were reset.

## Environment

Hostname: WindowsADClient

OS: Windows 10 Pro

Location/VPN: internal lab / domain-connected workstation

Related systems/services: Active Director , ADUC, domain authentication.

## Baseline Checks

Connectivity: Workstation was domain-connected

Account status: User account located in ADUC and available for reset.

Licensing/access: N/A

Error messages: “The user name or password is incorrect. Try again.”

Recent changes: None noted before reset action.

## Troubleshooting Performed

 Reproduced sign in failure at the workstation using the users prior known password.

 Located the FInance user in Active Directory Users and Computers.

 Rest password in ADUC and selected User must change password at next logon.

Confirmed the reset completed successfully

Logged back in with temporary password and completed the required password change.

Validated successful access after the password update.

## Findings

 Authentication issue was successfully reproduced at the workstation

Password reset workflow completed successfully in ADUC.

User was able to sign in after changing the password at next logon.

No additional account lockout, disablement, or workstation issue was observed.

## Root Cause

 User could not authenticate because the prior password was no longer valid/usable, requiring a standard password reset.

## Resolution

 Reset the Finance test user password in Active Directory

Forced password change at next logon.

User completed password update successfully and regained workstation access.

## Validation

User confirmed: Able to sign in after reset.

Functionality tested: Domain login completed successfully

Additional verification: Desktop session loaded successfully under the user account.

## Closure Notes
-Access restored through standard help desk password reset procedure.

-No escalation required.

-Ticket can be closed as resolved.

Last updated 5-12-26



