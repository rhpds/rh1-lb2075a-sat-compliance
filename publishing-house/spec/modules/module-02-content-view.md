# Module 02 — Create a content view for OpenSCAP repositories

## Brief Overview

This module creates the RHEL10 content view in Satellite — the foundational object that bundles the RHEL 10 repositories and the Satellite Client 6 repo required for OpenSCAP client installation. Participants navigate the Satellite Web UI to create the content view, add three repositories (BaseOS, AppStream, Satellite Client 6 for RHEL 10), publish a new version, and promote it to the Library lifecycle environment. This content view will be referenced by the activation key (module 03) and host group (module 05).

## Audience and Time

- **Target personas:** Systems administrators, Satellite administrators
- **Prerequisites for this module:** Satellite Web UI login complete (module 01); RHEL 10 repositories already synced (pre-provisioned)
- **Estimated duration:** 5 minutes

## Learning Objectives

- Create a content view in Red Hat Satellite that bundles multiple repositories
- Publish and promote a content view to a lifecycle environment
- Identify why a dedicated content view is required for managed host registration and OpenSCAP client availability

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Create a content view | 2 min |
| 2 | Add repositories to the content view | 2 min |
| 3 | Publish and promote | 1 min |

## Detailed Steps

1. In the Satellite Web UI, navigate to **Content > Content Views**.
2. Click **Create content view**.
3. Enter `RHEL10` as the name; leave the type as "Content View" (not composite).
4. Click **Create content view** to save.
5. In the newly created content view, click the **Repositories** tab.
6. Click **Add repositories**.
7. Filter by "RHEL 10" to locate the correct repos. Add all three:
   - Red Hat Enterprise Linux 10 BaseOS (RPMs)
   - Red Hat Enterprise Linux 10 AppStream (RPMs)
   - Red Hat Satellite Client 6 for RHEL 10 (RPMs)
8. Click **Add repositories** to confirm the selection.
9. Click **Publish new version**.
10. Accept the default version description and click **Next**, then **Finish**.
11. Wait for the publish task to complete (progress indicator in the task list).
12. Once published, confirm the version appears in the **Versions** tab and is promoted to **Library**.

## Key Takeaways

- Content views are Satellite's mechanism for controlling which package versions are available to managed hosts — they act as a snapshot of repository state at publish time
- The Satellite Client 6 repository must be included in the content view so managed hosts can install the `rubygem-foreman_scap_client` package via the activation key
- Publishing creates an immutable version; promoting makes it available in a lifecycle environment (Library = widest availability)

## Infrastructure Notes

- RHEL 10 BaseOS, AppStream, and Satellite Client 6 repositories must be synced before this module (pre-provisioned in setup automation)
- All steps are GUI-only; no CLI commands required
