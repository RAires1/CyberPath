# CyberPath

This repository documents my move into cybersecurity through hands-on labs, homelab projects and practical security work.

I use the lab to build experience with systems, networking, monitoring and incident analysis instead of only studying the theory.

## Current Focus

- Blue Team / Security Operations
- SIEM monitoring and event analysis
- Windows Server and Active Directory
- Networking and DNS
- Linux administration
- Virtualization
- Incident response and packet analysis

## Homelab

My main project is a Proxmox-based lab where I can build and troubleshoot a small enterprise-style environment.

Current components include:

- Proxmox VE virtualization
- OPNsense firewall/router
- Windows Server with Active Directory
- `aireslab.test` lab domain
- Windows 11 domain-joined workstation
- Ubuntu Server
- Wazuh SIEM with Windows and Linux agents
- AdGuard Home
- Home Assistant OS

I use the environment to practise domain administration, DNS, authentication, routing, endpoint monitoring, log collection and security-event investigation.

[View the Proxmox Homelab documentation](./Proxmox-Homelab/)

## Detections & Investigations

Beyond building the infrastructure, I document specific security events I've generated and investigated in the lab - what I did, what Wazuh detected, and how I analyzed it.

[View detection write-ups](./Proxmox-Homelab/Detections/)

## Security Training

- Google Cybersecurity Professional Certificate
- CompTIA Security+ - In Progress
- TryHackMe - 70+ rooms completed, Top 5%
- Blue Team and Security Operations labs
- Microsoft Sentinel hands-on lab work
- Phishing and packet-analysis exercises

## Roadmap

- **Sep 2026:** Deploy Sysmon on Windows endpoints for richer process/network telemetry; tune Wazuh rules against it.
- **Oct 2026:** Simulate an RDP brute-force attack, write a custom Wazuh detection rule, and document it as a full investigation.
- **Nov 2026:** Add file-integrity monitoring and a Linux authentication-abuse scenario.
- **Dec 2026:** Simulate a multi-stage attack chain across two endpoints and write a full incident-response report.

*(Dates are targets, not commitments - updated as the lab progresses.)*

## Connect

- [LinkedIn](https://www.linkedin.com/in/ricardo-da-cunha-aires-393255221/)
- [GitHub](https://github.com/RAires1)
