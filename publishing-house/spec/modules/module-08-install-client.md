# Module 08 — Install the OpenSCAP client via Ansible roles

## Brief Overview

This module demonstrates manual execution of the theforeman.foreman_scap_client Ansible role on rhel1.lab from the Satellite Web UI. In practice, the role runs automatically approximately 5 minutes after host registration; this module shows how to trigger it manually so participants can observe the job in real time. When the role runs, it installs the `rubygem-foreman_scap_client` package, writes the OpenSCAP client configuration file at `/etc/foreman_scap_client/config.yaml` (pointing to the Satellite capsule for ARF report upload), and creates a cron job that will execute scans on the schedule defined in the compliance policy. After this module, rhel1.lab is ready to accept scan commands from Satellite.

## Audience and Time

- **Target personas:** Systems administrators, Satellite administrators
- **Prerequisites for this module:** rhel1.lab and rhel2.lab registered to Satellite with Remote Execution enabled (module 07); theforeman.foreman_scap_client role assigned to RHEL10 host group (module 05)
- **Estimated duration:** 5 minutes

## Learning Objectives

- Trigger an Ansible role run on a managed host from the Red Hat Satellite Web UI
- Monitor the status of a Satellite Remote Execution job
- Verify that the OpenSCAP client package and cron job are installed on a managed host

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Run the Ansible role on rhel1.lab | 3 min |
| 2 | Check the status of the job | 2 min |

## Detailed Steps

1. In the Satellite Web UI, navigate to **Hosts > All Hosts**.
2. Click into **rhel1.lab** to open the host detail page.
3. Click the accordion next to **Schedule a job** and select **Run Ansible roles**.
4. In the job wizard, confirm the target host is rhel1.lab.
5. Click **Run** (or **Submit**) to start the Ansible role job.
6. The UI will redirect to the job detail page (or navigate to **Monitor > Jobs**).
7. Observe the job progress — the output will show Ansible task steps: package install (`rubygem-foreman_scap_client`), configuration file write (`/etc/foreman_scap_client/config.yaml`), and cron job creation for ARF report uploads.
8. Wait for rhel1.lab to report success (green status).
9. Optionally verify on rhel1.lab via the terminal tab:
   - Confirm package: `rpm -q rubygem-foreman_scap_client`
   - Confirm cron job: `crontab -l` or check `/etc/cron.d/` for foreman_scap_client entries
10. Feel free to run the role on rhel2.lab as well using the same steps if desired.

## Key Takeaways

- Satellite's Remote Execution framework allows Ansible roles to be pushed and executed on managed hosts without requiring direct SSH access from the operator — Satellite handles the connection using the SSH keys set up during registration
- The foreman_scap_client role configures the host to communicate with the Satellite capsule for ARF report upload, which is how compliance scan results flow back to the Satellite compliance dashboard
- Running Ansible roles from Satellite Web UI (rather than ad-hoc on the command line) creates an auditable job record, making it easy to confirm which hosts received the configuration and when

## Infrastructure Notes

- Remote Execution must be enabled on the hosts (configured during registration in module 07)
- The role applies the compliance policy configuration automatically — the cron job schedule is derived from the policy schedule set in module 06
- The role runs automatically ~5 minutes after host registration; this module demonstrates manual execution for observability
- All steps are GUI-only in Satellite Web UI; optional verification steps use the rhel1.lab terminal
