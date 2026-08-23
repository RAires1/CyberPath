# Detections & Investigations

This folder documents specific security events I generated and investigated in my Proxmox homelab, rather than just the infrastructure that produces them.

Each write-up follows the same format: what I did, what Wazuh detected, how I investigated it, and what it means in a real SOC context.

## Index

| Date | Title | Technique / Area | Status |
|---|---|---|---|
| 2026-08 | [Windows Failed Logon Detection](./2026-08-windows-failed-logon.md) | Authentication / Credential Access | Documented |
| 2026-08 | [PowerShell "Invoke-Command" Alert – False Positive Triage](./2026-08-powershell-false-positive-triage.md) | PowerShell / Lateral Movement (triage) | Documented |

More entries will be added as I run additional scenarios (RDP brute-force simulation, Sysmon-based process telemetry, Linux authentication abuse, multi-stage attack chain).

## Write-up Template

New entries follow [`TEMPLATE.md`](./TEMPLATE.md).
