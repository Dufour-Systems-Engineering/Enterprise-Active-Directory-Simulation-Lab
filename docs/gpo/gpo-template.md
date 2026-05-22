# GPO Template

**Last Updated:** May 19, 2026  
**Domain:** lab.local

## GPO Information

| Field                | Value                          |
|----------------------|--------------------------------|
| GPO Name             |                                |
| Type                 | User / Computer / Domain       |
| Target OU            |                                |
| Link Order           |                                |
| Security Filtering   |                                |
| WMI Filter           | None                           |

## Key Settings Applied

| Setting                          | Configured Value                          | Status     |
|----------------------------------|-------------------------------------------|------------|
|                                  |                                           |            |
|                                  |                                           |            |
|                                  |                                           |            |

## Verification Steps
- Run `gpupdate /force` on target client
- Verify with `gpresult /h gpresult.html` or RSOP.msc
- Check Event Viewer for Group Policy events

## Related Files
- [GPO Overview](gpo-overview.md)
- [Security Baseline](gpo-security-baseline.md)
- [Engineering Department Baseline](engineering-gpo-baseline.md)