# Red Hat Satellite Compliance

## Overview

Red Hat Satellite Compliance is a hands-on lab demonstrating how to use Red Hat Satellite 6.19's built-in compliance features to manage OpenSCAP scanning across RHEL 10 hosts. Participants configure Satellite to serve SCAP content, register RHEL hosts, deploy the OpenSCAP client via Ansible, run compliance scans against a DISA STIG profile, and remediate discovered violations — all within the Satellite Web UI and Hammer CLI. The lab covers the full compliance workflow end-to-end: from creating content views and activation keys, through host registration and client deployment, to running scans and applying Ansible-based remediation.

## Target Audience

- **Role:** Systems administrators, IT security professionals, Red Hat Satellite administrators
- **Experience level:** Intermediate
- **What they already know:** Basic Satellite Web UI navigation and administration; familiarity with RHEL system administration; root SSH access patterns; foundational security compliance concepts (what SCAP and STIG are)
- **What they don't know:** How to configure OpenSCAP compliance policies in Satellite; how to deploy the foreman_scap_client Ansible role via Satellite; how to interpret and remediate SCAP compliance reports using Satellite's built-in tooling

## Prerequisites

- Completion of a Red Hat Satellite Basics lab or equivalent hands-on Satellite administration experience (content view creation, repo sync, host registration fundamentals)
- Familiarity with RHEL system administration (systemd, package management, SSH)
- Root SSH access to managed hosts is pre-configured in the lab environment
- Cannot be fully auto-validated at intake — lab completion depends on Satellite Web UI state (GUI-driven steps) and repository sync status, which are observable but not fully scriptable without Satellite API calls

## Learning Objectives

1. Configure Red Hat Satellite compliance policies using OpenSCAP and SCAP Security Guide profiles
2. Manage RHEL host registration and content views in Red Hat Satellite
3. Analyze OpenSCAP compliance scan results and remediate violations using Satellite's Ansible-based remediation

## Content Type

Lab (hands-on)

## Products & Technologies

- Red Hat Satellite 6.19
- Red Hat Enterprise Linux 10
- OpenSCAP / SCAP Security Guide (SSG)
- SCAP (Security Content Automation Protocol)
- XCCDF compliance profiles: DISA STIG, OSPP, HIPAA, PCI-DSS, CIS
- Hammer CLI
- theforeman.foreman_scap_client Ansible role
- Red Hat Ansible Automation Platform (Ansible — via Satellite's built-in integration)
- Red Hat Insights
- Remote Execution

## Module Map

| Module | Title | Duration |
|--------|-------|----------|
| 1 | OpenSCAP Compliance Introduction | 5 min |
| 2 | Create a content view for OpenSCAP repositories | 5 min |
| 3 | Create an activation key | 5 min |
| 4 | Ingest and configure OpenSCAP content | 7 min |
| 5 | Configure a host group | 5 min |
| 6 | Create a compliance policy | 5 min |
| 7 | Register hosts to the RHEL10 host group | 5 min |
| 8 | Install the OpenSCAP client via Ansible roles | 5 min |
| 9 | Run an OpenSCAP scan on a host | 8 min |
| — | **Total hands-on** | **45 min** |
| — | Intro / presentation | ~0 min |
| — | **Total lab** | **~45 min** |

## Difficulty Level

Intermediate

## Environment

**Learner view:** The lab starts with a running Satellite 6.19 server (satellite.lab) with RHEL 10 repositories already synced from the Red Hat CDN. Two RHEL 10 managed hosts (rhel1.lab, rhel2.lab) are pre-deployed and reachable via SSH. SCAP Security Guide content and a custom DISA STIG SSHD tailoring file are pre-staged on disk. Participants access four UI surfaces: the Satellite Web UI, a satellite.lab terminal, a rhel1.lab terminal, and a rhel2.lab terminal.

**Automation needed:** Yes

Setup automation must provision:
- Satellite 6.19 server fully deployed with RHEL 10 BaseOS, AppStream, and Satellite Client 6 repositories pre-synced
- SCAP Security Guide content (SSG) pre-staged on the satellite.lab filesystem for Hammer upload
- DISA STIG SSHD tailoring file pre-staged for Hammer `tailoring-file create`
- Root SSH access pre-configured between the lab environment and rhel1.lab, rhel2.lab
- RHEL 10 managed hosts booted and reachable (but not yet registered to Satellite)

## Infrastructure Requirements

- **Cloud provider:** CNV (default)
- **Cluster type:** N/A — VM-based lab, no OpenShift
- **OCP version:** N/A
- **Topology:** Per-student (each student requires their own Satellite server and RHEL managed hosts; shared infrastructure is not viable due to Satellite's content view and host registration state)
- **Sizing:** TBD — confirmed in infrastructure phase
  - satellite.lab: Satellite 6.19 server (CPU/RAM/disk TBD — confirmed in infrastructure phase)
  - rhel1.lab: RHEL 10 managed host (CPU/RAM/disk TBD — confirmed in infrastructure phase)
  - rhel2.lab: RHEL 10 managed host (CPU/RAM/disk TBD — confirmed in infrastructure phase)
- **Automation approach:** Ansible
- **AI/MaaS:** None
- **External services:** Red Hat CDN — repositories are pre-synced at lab start; no active CDN connection is required during participant steps. Reference links to DISA/OpenSCAP documentation sites are included but not required for lab completion.
- **AAP version:** N/A — Ansible is used via Satellite's built-in Ansible integration, not a standalone AAP deployment
- **Non-GA products:** None (all products are GA)

## Assessment Strategy

The lab uses per-module solve and validation scripts matched to the Zero-Touch runtime automation framework. Because most modules are GUI-driven, validation relies on Satellite API checks rather than filesystem or CLI state.

- **module-01 (Introduction):** No automated validation needed — participants navigate to the Satellite Web UI and confirm they can log in. Solve script logs the URL and confirms HTTP 200.
- **module-02 (Content view):** Validation script calls the Satellite API to confirm the RHEL10 content view exists, has been published, and is promoted to the Library lifecycle environment.
- **module-03 (Activation key):** Validation confirms the RHEL10 activation key exists and has the Satellite Client 6 repository override enabled.
- **module-04 (OpenSCAP content):** Validation confirms SCAP content is uploaded (Hammer `scap-content list`), the foreman_scap_client Ansible role is imported, and the STIG SSHD tailoring file is present.
- **module-05 (Host group):** Validation confirms the RHEL10 host group exists with correct content view, lifecycle environment, Ansible role, and activation key assigned.
- **module-06 (Compliance policy):** Validation confirms the "DISA STIG no empty passwords ssh" compliance policy exists, is associated with the RHEL10 host group, and uses the tailoring file.
- **module-07 (Register hosts):** Validation confirms rhel1.lab and rhel2.lab appear as managed hosts in Satellite, subscribed and in the RHEL10 host group.
- **module-08 (Install client):** Validation confirms the rubygem-foreman_scap_client package is installed on both managed hosts and the cron job is present.
- **module-09 (Run scan — capstone):** Validation confirms compliance reports exist for both hosts in Satellite, at least one violation is reported, and the SSH empty-password remediation has been applied (PermitEmptyPasswords is set to no in sshd configuration).
