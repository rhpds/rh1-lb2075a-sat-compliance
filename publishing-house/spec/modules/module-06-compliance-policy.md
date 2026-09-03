# Module 06 — Create a compliance policy

## Brief Overview

This module creates the compliance policy that ties together the SCAP content, tailoring file, Ansible deployment method, schedule, and host group into a single enforceable policy object. Participants use the Satellite Web UI policy wizard to configure "DISA STIG no empty passwords ssh" — a targeted policy that scans for the SSH empty-password vulnerability using the Red Hat STIG XCCDF profile and the custom SSHD tailoring file created in module 04. The policy is associated with the RHEL10 host group so it applies to all hosts registered under that group.

## Audience and Time

- **Target personas:** Systems administrators, IT security professionals, Satellite administrators
- **Prerequisites for this module:** SCAP content uploaded (module 04), tailoring file created (module 04), RHEL10 host group configured (module 05)
- **Estimated duration:** 5 minutes

## Learning Objectives

- Create a compliance policy in Red Hat Satellite using the policy wizard
- Select an appropriate XCCDF profile and tailoring file for a targeted compliance scan
- Associate a compliance policy with a host group to define scan scope

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Launch the policy wizard and configure core settings | 3 min |
| 2 | Set schedule, host group, and finalize | 2 min |

## Detailed Steps

1. In the Satellite Web UI, navigate to **Hosts > Compliance > Policies**.
2. Click **New Policy**.
3. On the **Deployment options** step:
   - Select **Ansible** as the deployment method
   - Click **Next**
4. On the **Policy attributes** step:
   - Set **Name** to `DISA STIG no empty passwords ssh`
   - Click **Next**
5. On the **SCAP content** step:
   - Set **SCAP Content** to `Red Hat RHEL10 default content`
   - Set **XCCDF Profile** to `Red Hat STIG for Red Hat Enterprise Linux 10`
   - Set **Tailoring file** to `STIG SSHD Tailoring file` (created in module 04)
   - Set **XCCDF Profile in Tailoring File** to `DISA STIG for RHEL10 No Empty Passwords`
   - Click **Next**
6. On the **Schedule** step:
   - Set the period to **Weekly**
   - Explicitly select **Sunday** as the scan day
   - Click **Next**
7. On the **Locations** step:
   - Select **Vancouver** as the location
   - Click **Next**
8. On the **Organizations** step:
   - Select **Acme Org** as the organization
   - Click **Next**
9. On the **Host groups** step:
   - Add `RHEL10` to the selected host groups
   - Click **Submit** to create the policy
10. Confirm the policy `DISA STIG no empty passwords ssh` appears in the Compliance Policies list.

## Key Takeaways

- Compliance policies in Satellite combine what to scan (SCAP content + XCCDF profile), how to deploy the scanner (Ansible, Puppet, or manual), when to scan (schedule), and where to scan (host groups)
- Selecting Ansible as the deployment method means Satellite will use Remote Execution to push and run the OpenSCAP scanner on managed hosts — no agent pre-installation is needed beyond what the foreman_scap_client role provides
- Tailoring files allow a single STIG profile to be scoped to specific rules, reducing scan noise and focusing remediation effort on the highest-priority findings for the environment
- Red Hat's STIG profile (`Red Hat STIG for Red Hat Enterprise Linux 10`) is derived from the DISA STIG but may differ in timing and specific rule content; the tailoring file's profile (`DISA STIG for RHEL10 No Empty Passwords`) comes directly from the DISA benchmark

## Infrastructure Notes

- All steps are GUI-only via the Satellite Web UI policy wizard
- The tailoring file must already exist in Satellite (created in module 04) before this module
- The **XCCDF Profile in Tailoring File** field is a required selection in the wizard and must be set explicitly — it does not auto-populate from the tailoring file selection
