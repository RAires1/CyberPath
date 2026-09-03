# [Title]: [One-line summary]

**Date:** YYYY-MM
**Environment:** aireslab.test (Proxmox homelab)
**Technique / Area:** e.g. Credential Access, Persistence, Network Reconnaissance
**MITRE ATT&CK:** e.g. T1110: Brute Force (only include if it genuinely applies)

## Scenario

What I did and why. What was I trying to test or simulate?

## Environment

| Component | Detail |
|---|---|
| Source | e.g. WIN11-01 |
| Target | e.g. DC01 |
| Tooling | e.g. Wazuh, Sysmon |

## Steps Performed

1. Step-by-step account of the action taken on the endpoint.

## Detection

What Wazuh (or other tooling) actually showed. Include:

- Screenshot of the alert/dashboard
- Rule ID and rule description
- Relevant raw log snippet (with sensitive data redacted)

```text
<!-- TODO: paste the real Wazuh rule XML / alert JSON here -->
```

![Alert screenshot](./screenshots/REPLACE-ME.png)

## Analysis

Why the alert fired, what it means, and whether it correctly represents the activity. Note any false positives/negatives.

## Lessons Learned

What this taught me about the detection pipeline, the rule logic, or the underlying technology.

## Next Steps

How I'd extend this scenario (e.g. add a custom rule, correlate with another data source, build an active response).
