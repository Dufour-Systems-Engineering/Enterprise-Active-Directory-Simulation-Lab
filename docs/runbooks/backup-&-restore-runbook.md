# Backup and Restore Runbook

**Last Updated:** May 20, 2026  
**Purpose:** Documents the backup and recovery strategies used in the `lab.local` Azure-hosted environment, including decision rationale and future improvement plans.

## Backup Strategy Overview

In this Azure-based lab, the primary backup method used was **Azure Virtual Machine restore points**. This approach was chosen for speed and simplicity during active development.

---

## 1. Azure VM Restore Points (Primary Method Used)

Before major changes (such as bulk GPO deployments), named restore points were created on `windowsserver1`.

### Creation Procedure (Azure Portal)
1. Navigate to **Virtual Machines** → `windowsserver1` → **Restore Points** and prepare to create a new backup state.  
   * *See Evidence:* [01-restore-point-creation-before.png](../../screenshots/backup-and-restore/01-restore-point-creation-before.png)

2. Click **+ Create** and enter a descriptive, dated name...  
   * *See Evidence:* [02-restore-point-name-and-date.png](../../screenshots/backup-and-restore/02-restore-point-name-and-date.png)

3. Proceed to the validation stage...  
   * *See Evidence:* [03-restore-point-click-review-&-create.png](../../screenshots/backup-and-restore/03-restore-point-click-review-&-create.png)

4. Click **Create**...  
   * *See Evidence:* [04-restore-point-click-create-png.png](../../screenshots/backup-and-restore/04-restore-point-click-create-png.png)

5. Monitor the resource blade...  
   * *See Evidence:* [05-restore-point-verify-successful-deployment.png](../../screenshots/backup-and-restore/05-restore-point-verify-successful-deployment.png)

---

## 2. Design Decision & Future Planning

**Decision Rationale:** Azure VM restore points were selected as the primary backup method because they are the fastest and most reliable way to protect the entire server state in a cloud lab environment. They provided a quick safety net before risky operations.

**Future Plans:** While Azure restore points were sufficient for this phase, I intend to practice traditional **internal server backups** (Windows Server Backup with System State) in future iterations. This will allow me to gain hands-on experience with `wbadmin`, DSRM recovery, and full AD database backup/restore procedures — skills that are valuable in on-premises enterprise environments.

---

## 3. GPO Backups

Individual Group Policy Objects were backed up manually using the Group Policy Management Console before major changes.

---

## Lessons Learned
- Azure VM restore points are excellent for rapid recovery in cloud labs.
- Creating a named restore point immediately before risky changes is a critical safety practice.
- Practicing traditional AD backup methods remains an important future goal for deeper platform familiarity.

**Related Files:**
- [OU Management Runbook](OU-management-runbook.md)
- [GPO Overview](../gpo/gpo-overview.md)
