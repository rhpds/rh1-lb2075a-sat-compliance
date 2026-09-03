# Module 07 — Register hosts to the RHEL10 host group

## Brief Overview

This module registers the two RHEL 10 managed hosts (rhel1.lab and rhel2.lab) to Satellite under the RHEL10 host group. Participants use Hammer CLI on satellite.lab to generate a host registration script that includes the host group, Insights enrollment, and Remote Execution configuration. The generated script is then run over SSH on each host. After registration, both hosts appear in Satellite as managed hosts with the RHEL10 content view, Satellite Client package available, and Remote Execution enabled.

## Audience and Time

- **Target personas:** Systems administrators, Satellite administrators
- **Prerequisites for this module:** RHEL10 activation key (module 03), RHEL10 host group with OpenSCAP capsule and Ansible role (module 05), rhel1.lab and rhel2.lab reachable via SSH from satellite.lab
- **Estimated duration:** 5 minutes

## Learning Objectives

- Generate a host registration command using Hammer CLI
- Register RHEL hosts to Red Hat Satellite using the generated script
- Verify that hosts appear in Satellite under the correct host group with the expected configuration

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Generate the host registration command | 2 min |
| 2 | Register rhel1.lab and rhel2.lab | 2 min |
| 3 | Verify the registration | 1 min |

## Detailed Steps

1. Open the satellite.lab terminal tab.
2. Generate the host registration script using Hammer and store it directly as a shell variable:
   ```
   SAT_HOST_REGISTRATION_SCRIPT=$(hammer host-registration generate-command \
     --hostgroup "RHEL10" \
     --insecure true \
     --force true \
     --setup-insights true \
     --setup-remote-execution true)
   ```
3. Register rhel1.lab by running the script over SSH:
   ```
   ssh root@rhel1.lab bash -c "$SAT_HOST_REGISTRATION_SCRIPT"
   ```
4. Note: these commands will take a few minutes to complete — the output will show subscription, repository enablement, and Insights enrollment steps.
5. Register rhel2.lab:
   ```
   ssh root@rhel2.lab bash -c "$SAT_HOST_REGISTRATION_SCRIPT"
   ```
6. Wait for rhel2.lab registration to complete.
7. In the Satellite Web UI, navigate to **Hosts > All Hosts**.
8. Confirm both rhel1.lab and rhel2.lab appear in the host list with:
   - Host group: RHEL10
   - Content view: RHEL10
   - Lifecycle environment: Library
   - Subscription status: green

## Key Takeaways

- The Hammer `host-registration generate-command` workflow produces a curl-based registration script that includes all the configuration parameters (host group, Insights, Remote Execution) in a single command
- Running the registration script on a host installs the Satellite client tools, subscribes the host to the RHEL10 content view, and sets up SSH keys for Remote Execution
- Registering hosts to a host group (rather than registering without a group) means they inherit the group's OpenSCAP capsule assignment and Ansible role assignment, enabling the next two modules to work correctly
- The `--force true` flag forces re-registration, which is useful in lab environments where hosts may have been previously registered

## Infrastructure Notes

- Root SSH access from satellite.lab to rhel1.lab and rhel2.lab must be pre-configured (setup automation)
- The `--insecure true` flag is appropriate for lab environments; production environments should use proper TLS certificate validation
