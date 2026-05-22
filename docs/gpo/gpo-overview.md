# Group Policy Overview & Strategy

**Last Updated:** May 19, 2026  
**Domain:** lab.local

## Design Principles
The Group Policy architecture separates User and Computer configurations for better troubleshooting, reduced policy bloat, and improved logon performance. It follows Microsoft best practices while supporting departmental customization.

## Key decisions:
- Security filtering uses AGDLP groups instead of the default "Authenticated Users"
- Mitigation of MS16-072 vulnerability by removing Authenticated Users from filtering
- Domain Computers granted Read permission to ensure reliable policy application
- Preference for native GPO settings over registry hacks where practical
- Computer GPOs are linked at the `Workstations` OU level for broad machine coverage
- User GPOs are linked directly to departmental OUs under `Accounts`

## Implemented GPOs

| GPO Name              | Type     | Target OU / Link Location          | Security Filtering                     | Purpose                          |
|-----------------------|----------|------------------------------------|----------------------------------------|----------------------------------|
| Security-Baseline     | Domain   | Domain Root                        | Domain Computers                       | Core security hardening          |
| Engineering-Users     | User     | OU=Engineering,OU=Accounts         | GG_Engineering_Users                   | User experience & productivity   |
| SG-Engineering        | Computer | OU=Workstations                    | SG-Engineering + Domain Computers      | Computer configuration           |


## Related Files:
- [Security Baseline](gpo-security-baseline.md)
- [Engineering Department Baseline](engineering-gpo-baseline.md)
- [GPO Backup Manifest](gpo-backup-manifest.md)