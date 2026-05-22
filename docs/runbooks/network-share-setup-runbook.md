# Create CompanyShares Folder and SMB Share (GUI) Runbook

**Last Updated:** May 21, 2026  
**Purpose:** Step-by-step guide to install the File Server role/features and create the `CompanyShares` SMB share on `WindowsServer1` using the Server Manager GUI.

## Prerequisites
- Logged into `WindowsServer1` as Domain Admin (`LAB\david`)
- Server Manager is open

## Procedure

### 1. Install File Server Role and Required Features
1. In Server Manager, click **Add Roles and Features**.
2. Select **Role-based or feature-based installation**.
3. Select your server (`WindowsServer1`).
4. Under **Server Roles**, expand **File and iSCSI Services**.
5. Check the box for **File Server**.
6. Also select **DFS Namespaces** and **File Server Resource Manager**.
7. Click **Next** through the wizard and click **Install**.

**References:**  
[Screenshot: Add Roles and Features](../../screenshots/file-share-creation-&-mapping/01-add-roles-and-features.png)  
[Screenshot: Select Role-based Installation](../../screenshots/file-share-creation-&-mapping/02-select-role-based-installation.png)  
[Screenshot: Select Destination Server](../../screenshots/file-share-creation-&-mapping/03a-select-destination-server.png)  
[Screenshot: Select Destination Server (detail)](../../screenshots/file-share-creation-&-mapping/03b-select-destination-server.png)  
[Screenshot: File and iSCSI Services](../../screenshots/file-share-creation-&-mapping/04-select-file-and-iscsi-services.png)  
[Screenshot: Check File Server Box](../../screenshots/file-share-creation-&-mapping/05-check-file-server-box.png)  
[Screenshot: Select DFS Namespaces & Resource Manager](../../screenshots/file-share-creation-&-mapping/06-select-dfs-namespaces-and-resource-manager.png)  
[Screenshot: Click File Server Resource Manager](../../screenshots/file-share-creation-&-mapping/07-click-file-server-resource-manager.png)  
[Screenshot: Click Next and Install](../../screenshots/file-share-creation-&-mapping/08a-click-next-and-install.png)  
[Screenshot: Click Install](../../screenshots/file-share-creation-&-mapping/08b-click-next-and-install.png)

### 2. Create the CompanyShares SMB Share
1. In Server Manager, expand **File and Storage Services** in the left menu.
2. Click **Shares**.
3. In the top-right pane, click **Tasks** → **New Share…**.
4. Follow the **SMB Share – Quick** wizard:
   - Select **C:** drive.
   - Set the path to `C:\CompanyShares`.
   - Name the share **CompanyShares**.
   - Uncheck **Enable access-based enumeration** (see decision section below).
5. Accept default permissions for now (Everyone Read) — real security will be applied later inside the folder.
6. Click **Create**.

**References:**  
[Screenshot: File and Storage Services Left Menu](../../screenshots/file-share-creation-&-mapping/09-select-file-and-storage-services-left-menu.png)  
[Screenshot: Click Shares](../../screenshots/file-share-creation-&-mapping/10-click-shares.png)  
[Screenshot: Tasks → New Share](../../screenshots/file-share-creation-&-mapping/11a-tasks-new-share.png)  
[Screenshot: Tasks → New Share (detail)](../../screenshots/file-share-creation-&-mapping/11b-tasks-new-share.png)  
[Screenshot: Follow SMB Share – Quick Wizard](../../screenshots/file-share-creation-&-mapping/12a-follow-smb-share-quick-wizard-to-create.png)  
[Screenshot: Select Share Location & Path](../../screenshots/file-share-creation-&-mapping/12b-select-share-location-file-path.png)  
[Screenshot: Specify Share Name](../../screenshots/file-share-creation-&-mapping/12c-select-share-name-and-click-next.png)  
[Screenshot: Configure Share Settings](../../screenshots/file-share-creation-&-mapping/12d-configure-share-settings.png)  
[Screenshot: Uncheck Enable Access-Based Enumeration](../../screenshots/file-share-creation-&-mapping/12e-uncheck-enable-access-based-enumeration.png)  
[Screenshot: Specify Permissions](../../screenshots/file-share-creation-&-mapping/12f-specify-permissions-as-needed-and-click-next.png)  
[Screenshot: Click Create](../../screenshots/file-share-creation-&-mapping/12g-click-create.png)  
[Screenshot: CompanyShares is now live](../../screenshots/file-share-creation-&-mapping/13-companyshares-is-now-live.png)

## Design Decision – Single Company-Wide Share vs Per-Department Shares

A single company-wide share (`CompanyShares`) was intentionally created first instead of separate departmental shares.

**Rationale:**  
This approach was the fastest and simplest way to emulate a functional company file-sharing environment without getting bogged down in the complexity of setting up and securing individual departmental shares at this early stage. The goal was to maintain momentum during the lab build while still creating a realistic, usable file share structure.

Dedicated per-department shares will be implemented in a future phase once the core infrastructure is stable and the focus can shift to more granular access control.

## Share Configuration Decisions – Access-Based Enumeration (ABE)

During share creation, **Access-Based Enumeration (ABE)** was intentionally left **disabled**.

### Why ABE was disabled
- **Improved Server Performance**: When ABE is enabled, the server must dynamically evaluate permissions for every file and folder on every directory listing. This creates significant CPU and memory overhead.
- **Faster Response Times**: Disabling ABE eliminates real-time permission filtering, resulting in quicker navigation and reduced latency.
- **Simplified Troubleshooting and Administration**: Administrators can see the full folder hierarchy regardless of permissions, making auditing and management much easier.

### Trade-offs
Disabling ABE means users will see **all** folders and files on the share, even if they receive “Access Denied” when trying to open them. The performance and troubleshooting benefits were prioritized for this phase of the lab.

## Verification
- The share `CompanyShares` should now appear under Shares in Server Manager.
- You can now map drives from clients using `\\WindowsServer1\CompanyShares`.

## Related Files
- [GPO Overview](../gpo/gpo-overview.md)
