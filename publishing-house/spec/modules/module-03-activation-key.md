# Module 03 — Create an activation key

## Brief Overview

This module creates an activation key named RHEL10 in Satellite. The activation key ties a content view and lifecycle environment together, serving as the credential hosts use when registering to Satellite. Participants also enable the Satellite Client 6 repository override on the key so that managed hosts can install the OpenSCAP client package immediately upon registration. This activation key will be referenced by the host group (module 05) and used during host registration (module 07).

## Audience and Time

- **Target personas:** Systems administrators, Satellite administrators
- **Prerequisites for this module:** RHEL10 content view published and promoted to Library (module 02)
- **Estimated duration:** 5 minutes

## Learning Objectives

- Create an activation key in Red Hat Satellite and associate it with a content view and lifecycle environment
- Configure a repository override on an activation key to ensure specific repositories are enabled on registered hosts
- Explain the role of activation keys in the Satellite host registration workflow

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Create an activation key | 3 min |
| 2 | Enable the Satellite Client repository for the activation key | 2 min |

## Detailed Steps

1. In the Satellite Web UI, navigate to **Content > Lifecycle > Activation Keys**.
2. Click **Create activation key**.
3. Enter `RHEL10` as the name.
4. Click **Assign content view environments** to open the assignment modal.
5. Inside the modal, select the **Library** lifecycle environment.
6. Select the **RHEL10** content view created in module 02.
7. Click **Save**.
8. On the activation key detail page, click the **Repository Sets** tab.
9. Locate **Red Hat Satellite Client 6 for RHEL 10 x86_64 (RPMs)** in the repository list.
10. Select the radio button for this repository, then click **Select Action → Override to Enabled**.
11. Confirm that the activation key summary shows the RHEL10 content view and Library lifecycle environment, and that Satellite Client 6 is listed as enabled in the Repository Sets tab.

## Key Takeaways

- Activation keys combine a content view, lifecycle environment, and repository enablement into a single registration credential
- The repository override ensures the Satellite Client 6 repo is automatically enabled on hosts that register using this key, without requiring manual `subscription-manager repos --enable` commands
- Activation keys are referenced by both host groups (for default assignment) and the host registration script (for runtime use)

## Infrastructure Notes

- All steps are GUI-only; no CLI commands required
- The activation key name `RHEL10` is referenced by name in subsequent modules — ensure the name matches exactly
