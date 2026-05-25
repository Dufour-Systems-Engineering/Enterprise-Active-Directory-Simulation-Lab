# Active Directory Domain Services (AD DS) Lab Portfolio

**Windows Server 2025 • Azure • lab.local**

This repository contains a complete, production-style documentation portfolio of a realistic mid-sized enterprise Active Directory Domain Services homelab built entirely in Azure. It demonstrates enterprise-grade design decisions, best practices, and hands-on administration skills through detailed runbooks, visual evidence, and real-world implementation.

The project was built to simulate real corporate Active Directory Domain Services environments, including scalable OU design, AGDLP group strategy, large-scale user provisioning (~1,000 users), automated Group Policy deployment, delegated administration, file share configuration, backup/restore procedures, and Azure break/fix troubleshooting scenarios.

### Key Achievements Demonstrated

- **Architecture** — Scalable OU hierarchy design with departmental separation, delegation boundaries, and full AGDLP group nesting model for least-privilege access control, supported by clear diagrams and detailed design rationale.
- **Delegation** — Hands-on least-privilege delegation for Tier 1 Help Desk accounts, including positive/negative testing, cross-OU user moves, and the “Three Locks” troubleshooting methodology.
- **GPO Management** — Comprehensive Group Policy implementation featuring pre-flight verification, manual GUI configuration (Sales), and full automated PowerShell rollout (Engineering) for wallpaper, drive mapping, folder redirection, taskbar layout, and security baselines.
- **Troubleshooting** — Real-world break/fix scenarios covering password resets, authentication failures, share permission issues, and GPO scoping problems — fully documented with root cause analysis and resolution steps.
- **Runbooks** — Detailed, step-by-step runbooks covering OU management, group management, bulk user import, domain join, network share setup, backup & restore, and more — all with screenshots, verification commands, lessons learned, and real-world context.

### 📁 Project Structure & Navigation

* **[architecture/](architecture/)** — Conceptual domain design blueprints, infrastructure mapping, and visual topology.
* **[docs/](docs/)** — Core engineering documentation
  * **[runbooks/](docs/runbooks/README.md)** — (Recommended starting point) Step-by-step administrative execution guides, logic verification, and project roadmaps.
  * **[delegation/](./docs/delegation/)** — Two-tier Help Desk & IT Admin delegation model, testing results, and future expansion plans.
  * **[gpo/](docs/gpo/)** — Baseline policy configurations, registry overrides, and client enforcement rulesets.
  * **[troubleshooting/](./docs/troubleshooting/)** — Real break/fix scenarios and resolutions (e.g., RDP access issues, permission problems). *(6 articles published as of May 25, 2026 — actively expanding)*.
* **[evidence/](evidence/)** — Supporting artifacts and raw documentation:
  * `raw-tickets/` — Original raw SDESK service desk tickets
  * `jira-project-export-pdfs/` — Exported Jira tickets and project data
* **[screenshots/](screenshots/)** — Organized visual evidence of every major configuration step.
* **[scripts/](scripts/)** — Reusable PowerShell automation modules and account ingestion loops *(content prepared and being polished)*.
* **[CHANGELOG.md](CHANGELOG.md)** — Comprehensive record of project updates, environment modifications, and version controls.

### Current Status & Roadmap

**This portfolio is actively maintained.**  
New runbooks, evidence files, scripts, and troubleshooting articles are added as they are completed to show real-time progress to recruiters and hiring managers.

**Completed as of May 25, 2026:**
- Architecture folder
- Delegation folder (3 documents)
- GPO folder
- Runbooks folder (9 completed runbooks)
- Troubleshooting folder (6 polished articles)
- Evidence folder structure with raw tickets and Jira exports

**In Progress / Content Ready:**
- Evidence folder (raw tickets, Jira exports, and new subfolders being populated)
- Remaining runbooks (Engineering User Onboarding, Tier 1 Support Delegation, User Onboarding)
- Additional troubleshooting articles
- Full `scripts/` folder

Every new file will be committed immediately upon completion so you can watch the portfolio grow live.

**Last Updated:** May 25, 2026  
**Status:** Initial public release + Troubleshooting & Evidence sections launched

---

**Quick Start**  
Start here: **[Runbooks Index](docs/runbooks/README.md)**

This repository was built as a self-directed project to demonstrate the ability to design, implement, document, and troubleshoot enterprise-scale Active Directory environments.

[CHANGELOG](CHANGELOG.md) • [Runbooks Index](docs/runbooks/README.md)