# Module 04 — Ingest and configure OpenSCAP content

## Brief Overview

This is the most CLI-intensive module in the lab. Participants use the Hammer CLI on satellite.lab to upload SCAP Security Guide content to Satellite, import the theforeman.foreman_scap_client Ansible role and its variables, and create a custom DISA STIG SSHD tailoring file. These three assets are the building blocks for the compliance policy created in module 06: the SCAP content defines what to scan for, the Ansible role installs the scanning client on managed hosts, and the tailoring file narrows the STIG profile to the specific SSH empty-password rule used in this lab.

## Audience and Time

- **Target personas:** Systems administrators, Satellite administrators comfortable with CLI
- **Prerequisites for this module:** Satellite up and functional; SCAP Security Guide content and SSHD tailoring file pre-staged on the satellite.lab filesystem (provisioned by setup automation)
- **Estimated duration:** 7 minutes

## Learning Objectives

- Use Hammer CLI to upload SCAP content to Red Hat Satellite
- Import an Ansible role and its variables into Satellite using Hammer
- Create a custom SCAP tailoring file in Satellite to narrow a compliance profile to specific rules

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Upload the OpenSCAP content | 2 min |
| 2 | Import the OpenSCAP client Ansible role and variables | 3 min |
| 3 | Import the STIG SSHD tailoring file | 2 min |

## Detailed Steps

1. Open the satellite.lab terminal tab.
2. Run the SCAP content bulk upload command to load the SCAP Security Guide content into Satellite:
   ```
   hammer scap-content bulk-upload --type default
   ```
3. Navigate to **Hosts > Compliance > SCAP contents** in the Satellite Web UI to confirm the upload — the RHEL 10 SCAP content entries should now appear in the list.
4. Import the `theforeman.foreman_scap_client` Ansible role from the Satellite capsule into Satellite's role library:
   ```
   hammer ansible roles import --proxy-id 1 --role-names theforeman.foreman_scap_client
   ```
5. Confirm the output includes `theforeman.foreman_scap_client` in the list of imported roles.
6. Import the Ansible variables associated with the role so Satellite can expose them for customization in host groups:
   ```
   hammer ansible variables import --proxy-id 1
   ```
7. Confirm the variables list is populated (several foreman_scap_client-prefixed variables should appear).
8. Create the custom STIG SSHD tailoring file in Satellite, pointing to the pre-staged tailoring XML file:
   ```
   hammer tailoring-file create \
     --name "STIG SSHD Tailoring file" \
     --scap-file /tmp/rhel10-stig-sshd-tailoring.xml \
     --organizations "Acme Org" \
     --locations "Vancouver"
   ```
9. Confirm the command completes without error and note the tailoring file ID returned.

## Key Takeaways

- Satellite stores SCAP content centrally and distributes it to managed hosts at scan time — administrators do not need to stage content on each host individually
- The foreman_scap_client Ansible role is the bridge between Satellite compliance policies and the managed host: it installs the client, writes the policy configuration, and creates the cron job for ARF report upload
- Tailoring files allow compliance teams to customize XCCDF profiles by enabling or disabling specific rules, rather than using a monolithic profile — this is how the lab focuses on the single SSH empty-password rule from the full STIG

## Infrastructure Notes

- SCAP Security Guide RPM must be installed on satellite.lab (pre-provisioned by setup automation)
- STIG SSHD tailoring file must be pre-staged at `/tmp/rhel10-stig-sshd-tailoring.xml` by setup automation
- `proxy-id 1` assumes the default Satellite capsule ID; verify with `hammer capsule list` if needed
- This module uses the satellite.lab terminal; no access to rhel1.lab or rhel2.lab required
