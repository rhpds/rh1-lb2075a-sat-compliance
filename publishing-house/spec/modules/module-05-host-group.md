# Module 05 — Configure a host group

## Brief Overview

This module creates the RHEL10 host group in Satellite — a container object that bundles all the configuration elements built in previous modules (content view, lifecycle environment, activation key, OpenSCAP capsule, and Ansible role) into a single assignable unit. When hosts are registered to this host group (module 07), they inherit all of these settings automatically. The host group is also referenced by the compliance policy (module 06) to define which hosts fall under the policy's scope.

## Audience and Time

- **Target personas:** Systems administrators, Satellite administrators
- **Prerequisites for this module:** RHEL10 content view (module 02), RHEL10 activation key (module 03), foreman_scap_client Ansible role imported (module 04)
- **Estimated duration:** 5 minutes

## Learning Objectives

- Create a host group in Red Hat Satellite and configure it with a content view, lifecycle environment, and activation key
- Assign an OpenSCAP capsule and Ansible role to a host group to enable centralized compliance management
- Explain how host groups simplify host configuration by encoding policy defaults

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Create the host group and set core properties | 3 min |
| 2 | Assign Ansible role and activation key | 2 min |

## Detailed Steps

1. In the Satellite Web UI, navigate to **Configure > Host Groups**.
2. Click **Create host group**.
3. On the main **Host Group** tab:
   - Set **Name** to `RHEL10`
   - Set **Content Source** to `satellite.lab`
   - Set **Lifecycle environment** to `Library`
   - Set **Content view** to `RHEL10`
   - Set **OpenSCAP capsule** to `satellite.lab` (the default capsule)
4. Click the **Ansible Roles** tab:
   - Add `theforeman.foreman_scap_client` to the list of assigned roles
5. Click the **Activation Keys** tab:
   - Add `RHEL10` as the activation key
6. Click **Submit** to save the host group.
7. Confirm the RHEL10 host group appears in the list with the correct lifecycle environment and content view displayed.

## Key Takeaways

- Host groups are a Satellite best practice for managing groups of hosts with identical configurations — they eliminate the need to configure each host individually
- Assigning the OpenSCAP capsule at the host group level determines which Satellite capsule hosts in this group will use for uploading ARF compliance reports
- Assigning the foreman_scap_client Ansible role to the host group means all hosts in the group can have the role applied in a single Satellite job (done in module 08)

## Infrastructure Notes

- All steps are GUI-only; no CLI commands required
- The host group name `RHEL10` is referenced by the compliance policy in module 06 — ensure the name matches exactly
- The OpenSCAP capsule drop-down lists registered capsules; satellite.lab should be the only entry in a single-capsule lab environment
