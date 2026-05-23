# CHANGELOG

## [2026-05-23] - Troubleshooting Section Launch + Evidence Organization

### Added
- Launched the **Troubleshooting** section with 5 polished articles following the **AD Portfolio Runbook Template (APRT)**:
  - `sdesk-1-ad-password-reset-finance-user.md`
  - `sdesk-2-finance-user-workstation-authentication-failure.md`
  - `sdesk-3-ad-share-access-denied.md`
  - `sdesk-4-eng-dept-share-folder-rw-permission-issue.md`
  - `sdesk-5-finance-control-panel-restriction-gpo-scope-mismatch-report.md`

- Fully structured the **Evidence** folder:
  - `raw-tickets/` — Contains the 5 original raw SDESK service desk tickets
  - `jira-project-export-pdfs/` — Contains the 5 corresponding Jira export PDFs

- Created dedicated screenshot folders under `screenshots/troubleshooting/` for each SDESK issue.

### Improvements
- All troubleshooting documents now follow consistent professional structure with visual evidence and lessons learned.
- Established organized supporting artifact structure for raw tickets and Jira exports.

**Status:** Troubleshooting and Evidence sections now active and organized.


## [2026-05-22] – Initial Public Release (v1.0 Baseline)

**Added:**
- Root-level `README.md` and this `CHANGELOG.md`
- Complete `docs/runbooks/` folder with 9 fully completed, production-ready runbooks
- `docs/runbooks/README.md` as the official runbooks index
- Full folder structure with `architecture/`, `docs/`, `evidence/`, `screenshots/`, and `scripts/` at the root level
- Populated content in:
  - `architecture/` — OU hierarchy, AGDLP design, and visual diagrams
  - `docs/delegation/` — Delegation overview and testing results
  - `docs/gpo/` — Group Policy configurations and automation
- Comprehensive visual evidence in the `screenshots/` folder
- Initial framework for `evidence/`, `scripts/`, and `troubleshooting/` folders (content prepared and being polished)

**Purpose of this release:**  
This is the first public commit of the AD DS Lab portfolio. It showcases a realistic mid-sized enterprise Active Directory Domain Services environment built in Azure, with clear documentation of design decisions, implementation steps, and verification.

**Currently Complete:** 9 runbooks + major supporting folders  
**Status:** Initial public baseline — live progress tracking begins today.

## [Unreleased]
- Remaining runbooks (Engineering User Onboarding, Tier 1 Support Delegation, User Onboarding)
- Full population and polishing of `evidence/`, `scripts/`, and `troubleshooting/` folders
- Final Related Files cross-linking sweep across all documents
---

**How future entries will appear:**  
Each new completed runbook or major addition will receive its own dated entry so recruiters and hiring managers can see real-time development of the portfolio.