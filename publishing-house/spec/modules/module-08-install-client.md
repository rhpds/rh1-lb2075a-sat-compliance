# Module 08 — Install the OpenSCAP client via Ansible roles

## Brief Overview

This module deploys the OpenSCAP scanning client to both managed hosts by triggering the theforeman.foreman_scap_client Ansible role from the Satellite Web UI. When the role runs, it installs the `rubygem-foreman_scap_client` package, writes the OpenSCAP client configuration file (pointing to the Satellite capsule for ARF report upload), and creates the cron job that will execute scans on the schedule defined in the compliance policy. After this module, both hosts are ready to accept scan commands from Satellite.

## Audience and Time

- **Target personas:** Systems administrators, Satellite administrators
- **Prerequisites for this module:** rhel1.lab and rhel2.lab registered to Satellite with Remote Execution enabled (module 07); theforeman.foreman_scap_client role assigned to RHEL10 host group (module 05)
- **Estimated duration:** 5 minutes

## Learning Objectives

- Trigger an Ansible role run on managed hosts from the Red Hat Satellite Web UI
- Monitor the status of a Satellite Remote Execution job
- Verify that the OpenSCAP client package and cron job are installed on managed hosts

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Run the Ansible role on managed hosts | 3 min |
| 2 | Check the status of the job | 2 min |

## Detailed Steps

1. In the Satellite Web UI, navigate to **Hosts > All Hosts**.
2. Select both rhel1.lab and rhel2.lab using the checkboxes.
3. From the **Actions** drop-down (or "Select Action"), choose **Assign Ansible roles** (if not already assigned) — confirm theforeman.foreman_scap_client is listed.
4. With both hosts selected, click **Actions > Run all Ansible roles** (or navigate to **Configure > Ansible > Run roles**).
5. In the job wizard, confirm the target hosts include both rhel1.lab and rhel2.lab.
6. Click **Run** (or **Submit**) to start the Ansible role job.
7. The UI will redirect to the job detail page (or navigate to **Monitor > Jobs**).
8. Observe the job progress — the output will show Ansible task steps: package install, config file write, cron job creation.
9. Wait for both hosts to report success (green status).
10. Optionally verify on rhel1.lab via the terminal tab:
    - Confirm package: `rpm -q rubygem-foreman_scap_client`
    - Confirm cron job: `crontab -l` or check `/etc/cron.d/` for foreman_scap_client entries

## Key Takeaways

- Satellite's Remote Execution framework allows Ansible roles to be pushed and executed on managed hosts without requiring direct SSH access from the operator — Satellite handles the connection using the SSH keys set up during registration
- The foreman_scap_client role configures the host to communicate with the Satellite capsule for ARF report upload, which is how compliance scan results flow back to the Satellite compliance dashboard
- Running Ansible roles from Satellite Web UI (rather than ad-hoc on the command line) creates an auditable job record, making it easy to confirm which hosts received the configuration and when

## Infrastructure Notes

- Remote Execution must be enabled on the hosts (configured during registration in module 07)
- The role applies the compliance policy configuration automatically — the cron job schedule is derived from the policy schedule set in module 06
- All steps are GUI-only in Satellite Web UI; optional verification steps use the rhel1.lab terminal
