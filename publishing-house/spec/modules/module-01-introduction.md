# Module 01 — OpenSCAP Compliance Introduction

## Brief Overview

This module orients participants to the OpenSCAP ecosystem and explains how Red Hat Satellite 6.19 integrates with SCAP-based compliance workflows. It covers the key components — SCAP, XCCDF profiles (DISA STIG, CIS, HIPAA, PCI-DSS, OSPP), the SCAP Security Guide (SSG), and Satellite's compliance subsystem — before walking participants through their first interaction with the lab environment: logging into the Satellite Web UI. The module sets expectations for the full nine-module lab flow so participants understand where each subsequent step fits.

## Audience and Time

- **Target personas:** Systems administrators, IT security professionals, Satellite administrators
- **Prerequisites for this module:** None — this is the entry point
- **Estimated duration:** 5 minutes

## Learning Objectives

- Identify the components of the SCAP framework (XCCDF, OVAL, ARF) and how they relate to compliance scanning
- Describe how Red Hat Satellite integrates with OpenSCAP to centralize compliance policy management across RHEL hosts
- Log in to the Satellite 6.19 Web UI and navigate the compliance sections

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | What is OpenSCAP? | 2 min |
| 2 | Introduction to Red Hat Satellite 6.19 Compliance | 2 min |
| 3 | Log into the Web UI | 1 min |

## Detailed Steps

1. Read the OpenSCAP overview: understand SCAP as a NIST standard; identify XCCDF (checklists), OVAL (vulnerability definitions), and ARF (result format) as the three core components.
2. Review the available XCCDF profiles relevant to this lab: DISA STIG (the profile used throughout), and the other available profiles (CIS, HIPAA, PCI-DSS, OSPP) — understand that Satellite supports all of them.
3. Understand the lab topology: satellite.lab running Satellite 6.19, rhel1.lab and rhel2.lab as managed RHEL 10 hosts.
4. Open the Satellite Web UI URL in the browser tab.
5. Log in using the provided admin credentials.
6. Confirm the Satellite dashboard loads and note the navigation menu — observe the Hosts > Compliance section which will be used in later modules.
7. Review the high-level lab workflow: content view (module 02) → activation key (module 03) → OpenSCAP content (module 04) → host group (module 05) → compliance policy (module 06) → host registration (module 07) → client install (module 08) → scan and remediate (module 09).

## Key Takeaways

- SCAP is a standardized protocol for expressing and measuring security posture; OpenSCAP is the open-source implementation used on RHEL
- The SCAP Security Guide (SSG) provides pre-built XCCDF profiles for common compliance frameworks; Satellite distributes and manages these profiles centrally
- Satellite's compliance workflow separates content management (what to scan for) from policy enforcement (when and on which hosts to scan)
- The lab uses DISA STIG as the compliance profile, with a custom tailoring file that restricts the profile to the SSH empty-password rule

## Infrastructure Notes

- satellite.lab Web UI is accessible via the Satellite Web UI tab in the showroom
- Admin credentials are pre-provisioned in the lab environment
- No CLI commands in this module — GUI only
