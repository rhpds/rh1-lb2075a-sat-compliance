# Module 06 — Create a compliance policy

## Brief Overview

This module creates the compliance policy that ties together the SCAP content, tailoring file, Ansible deployment method, schedule, and host group into a single enforceable policy object. Participants use the Satellite Web UI policy wizard to configure "DISA STIG no empty passwords ssh" — a targeted policy that scans for the SSH empty-password vulnerability using the DISA STIG XCCDF profile and the custom SSHD tailoring file created in module 04. The policy is associated with the RHEL10 host group so it applies to all hosts registered under that group.

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
2. Click **New Compliance Policy**.
3. On the **Deployment options** step:
   - Select **Ansible** as the deployment method
   - Click **Next**
4. On the **Policy attributes** step:
   - Set **Name** to `DISA STIG no empty passwords ssh`
   - Click **Next**
5. On the **SCAP content** step:
   - Select the SCAP content entry that matches the RHEL 10 STIG data stream (e.g., `ssg-rhel10-ds.xml`)
   - Set **XCCDF Profile** to `DISA STIG for Red Hat Enterprise Linux 10` (or the STIG profile in the list)
   - Set **Tailoring file** to `RHEL10 STIG sshd` (created in module 04)
   - Set **Tailoring file profile** to the STIG sshd profile within the tailoring file
   - Click **Next**
6. On the **Schedule** step:
   - Set the period to **Weekly**
   - Accept the default day and time, or set a specific scan window
   - Click **Next**
7. On the **Locations** step:
   - Confirm the default location is selected
   - Click **Next**
8. On the **Organizations** step:
   - Confirm the default organization is selected
   - Click **Next**
9. On the **Host groups** step:
   - Add `RHEL10` to the selected host groups
   - Click **Submit** to create the policy
10. Confirm the policy `DISA STIG no empty passwords ssh` appears in the Compliance Policies list.

## Key Takeaways

- Compliance policies in Satellite combine what to scan (SCAP content + XCCDF profile), how to deploy the scanner (Ansible, Puppet, or manual), when to scan (schedule), and where to scan (host groups)
- Selecting Ansible as the deployment method means Satellite will use Remote Execution to push and run the OpenSCAP scanner on managed hosts — no agent pre-installation is needed beyond what the foreman_scap_client role provides
- Tailoring files allow a single DISA STIG profile to be scoped to specific rules, reducing scan noise and focusing remediation effort on the highest-priority findings for the environment

## Infrastructure Notes

- All steps are GUI-only via the Satellite Web UI policy wizard
- The tailoring file must already exist in Satellite (created in module 04) before this module
- The XCCDF profile name in the UI may vary slightly depending on the SSG version — select the profile with "STIG" in its name for RHEL 10
