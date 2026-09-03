# Windows Failed Logon Detection: Validating the Endpoint-to-SIEM Pipeline

**Date:** 2026-08
**Environment:** aireslab.test (Proxmox homelab)
**Technique / Area:** Authentication / Credential Access
**MITRE ATT&CK:** T1110: Brute Force (single deliberate failed attempt, not a full brute-force run, see Next Steps)

## Scenario

Both endpoints (`WIN11-01` and the Ubuntu Server) were showing as active in Wazuh, but an "active" agent isn't proof the SIEM is actually receiving useful security telemetry. I wanted a concrete, repeatable test that would let me trace one action all the way through to an alert in the dashboard, so I deliberately entered an incorrect password on the domain-joined Windows 11 client (`WIN11-01`) while authenticating against `DC01`.

## Environment

| Component | Detail |
|---|---|
| Source of the logon attempt | WIN11-01 (Windows 11, domain-joined) |
| Domain controller | DC01 (aireslab.test) |
| Wazuh manager | wazuh01 |
| Detecting agent | DC01 (agent.id: 001) |
| Data source | Windows Security event log via Wazuh agent on DC01 |

## Steps Performed

1. Logged out of the domain session on WIN11-01.
2. Attempted to authenticate with a deliberately incorrect password against the `aireslab.test` domain.
3. Confirmed the failed attempt locally (logon rejected on the client).
4. Switched to the Wazuh Threat Hunting view, filtered to `agent.id: 001` (DC01) and the `Authentication failure` rule group.
5. Verified the failed logon appeared as an alert.

## Detection

Wazuh generated 2 hits for the test window, both logged against `DC01` rather than the client itself. Domain authentication is processed and logged by the domain controller, so that's where the Security event and the Wazuh alert show up, not on WIN11-01.

| Field | Value |
|---|---|
| rule.id | 60131 |
| rule.description | Windows DC Logon Failure |
| rule.level | 5 |
| agent.name | DC01 |
| manager.name | wazuh01 |

![Wazuh failed logon alert](./screenshots/wazuh-failed-logon-alert.png)

## Analysis

This gave a simple but complete end-to-end trace:

```text
Deliberate bad-password logon attempt on WIN11-01
      |
      v
DC01 processes the domain authentication request and rejects it
      |
      v
Windows Security event (logon failure) written on DC01
      |
      v
Wazuh agent on DC01 forwards the event
      |
      v
Rule 60131 (Windows DC Logon Failure) matches and fires
      |
      v
Alert visible and searchable in the Wazuh dashboard
```

The value here wasn't the individual alert, because a single bad password isn't an attack. It was proving that every link in the pipeline (event generation, agent forwarding, rule matching, dashboard visibility) actually works. That's a prerequisite for trusting any later, more realistic detection scenario in this lab.

## Lessons Learned

- An agent showing "active" in Wazuh only confirms connectivity, not that useful telemetry is flowing. I had to generate a real event and confirm it arrived.
- I initially assumed the alert would be tied to the client (WIN11-01) since that's where the bad password was typed. It's actually tied to the domain controller, because DC01 is what processes and rejects the authentication request and writes the Security event. Testing this assumption, rather than just guessing, is what made the exercise useful.
- Time-window and field filtering in the Wazuh dashboard matters; without filtering to the right agent/rule group, the same events are much harder to find.
- Understanding *which* Windows Security Event ID maps to a logon failure (and how Wazuh's default ruleset, rule 60131 in this case, classifies it) is a separate skill from just deploying the agent.

## Next Steps

- Repeat this as an actual brute-force simulation (multiple rapid failed attempts from one source) to see whether the default Wazuh ruleset correlates them into a higher-severity alert, and compare against a single isolated failure like this one.
- Add a custom Wazuh rule/decoder for a scenario not covered by the default ruleset.
- Correlate the Windows-side alert with OPNsense firewall logs for the same time window once firewall log forwarding is in place.
