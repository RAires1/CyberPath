# Wazuh SIEM – Windows & Linux Monitoring in My Proxmox Homelab

## Overview

I deployed **Wazuh** in my Proxmox cybersecurity lab to create a centralized place for collecting and reviewing security events from both Windows and Linux endpoints.

The environment currently monitors:

- A Windows 11 domain-joined workstation
- An Ubuntu Server endpoint

This project connects the infrastructure side of my homelab with the security side. Instead of only building systems, I can now generate activity on those systems and investigate what appears in the SIEM.

---

## Environment

| Component | Configuration |
|---|---|
| Hypervisor | Proxmox VE |
| SIEM | Wazuh |
| Windows Endpoint | WIN11-01 |
| Linux Endpoint | Ubuntu Server |
| Windows Domain | aireslab.test |
| Monitoring | Windows and Linux agents |
| Status | Active |

I do not publish credentials, real internal addressing or sensitive logs from my environment.

---

## Why I Built This

I wanted to move beyond isolated security labs and start monitoring systems that I actually built and control.

Wazuh gives me a way to practise:

- Endpoint monitoring
- Centralized logging
- Alert review
- Authentication-event analysis
- Windows and Linux agent management
- Basic SIEM workflows

---

## Simplified Architecture

```text
                +--------------------+
                |      Wazuh SIEM    |
                | Dashboard / Server |
                +----------+---------+
                           |
              +------------+------------+
              |                         |
              ▼                         ▼
      +---------------+         +---------------+
      | WIN11-01      |         | Ubuntu Server |
      | Windows 11    |         | Linux         |
      | Wazuh Agent   |         | Wazuh Agent   |
      +---------------+         +---------------+
```

---

## Agent Deployment

I installed Wazuh agents on both Windows and Ubuntu and verified that both endpoints appeared as active in the Wazuh dashboard.

The general workflow was:

1. Prepare the endpoint.
2. Install the Wazuh agent.
3. Configure it to communicate with the Wazuh server.
4. Start the agent service.
5. Verify connectivity.
6. Confirm the endpoint appears as active in the dashboard.
7. Generate test activity.
8. Review resulting alerts and events.

---

## Windows Authentication Testing

One of my first useful detections came from deliberately entering incorrect credentials on the Windows endpoint.

Wazuh generated a logon-failure alert showing an unknown user or bad password condition.

This was useful because I could directly connect:

```text
Action on endpoint
      |
      ▼
Windows security event
      |
      ▼
Wazuh agent
      |
      ▼
Wazuh rule / alert
      |
      ▼
SIEM investigation
```

This gave me a simple but real end-to-end example of how endpoint activity becomes a security alert.

---

## Validation and Troubleshooting

After deploying both agents, I did not treat an "active" status as the end of the work.

I also tested whether useful activity actually appeared in the dashboard.

When events did not appear immediately, I checked:

- Agent status
- Service status
- Network connectivity
- Wazuh server communication
- Time window in the dashboard
- Whether the test action generated the type of event I expected

This helped me understand the difference between an agent being connected and the SIEM actually receiving useful security telemetry.

---

## What I Use the Lab For

Current exercises include:

- Failed Windows logons
- Authentication-event review
- Windows endpoint monitoring
- Linux endpoint monitoring
- Comparing events between operating systems
- Checking agent health
- Learning how Wazuh rules classify activity

---

## What I Learned

This project gave me practical experience with:

- Wazuh
- SIEM concepts
- Windows and Linux agents
- Centralized logging
- Endpoint telemetry
- Security alerts
- Failed-logon investigation
- Windows authentication events
- Troubleshooting monitoring pipelines

One of the most useful lessons was that building a SIEM is not only about installing the dashboard. The endpoints, network path, agent services, logs and detection rules all have to work together.

---

## Future Improvements

I plan to expand the monitoring environment with:

- Sysmon
- More detailed Windows telemetry
- Linux authentication exercises
- File-integrity monitoring
- Additional Wazuh rules and use cases
- Network-security events
- Controlled incident-response scenarios
- Correlating activity across multiple endpoints

Any attack simulation will be performed only inside my own controlled homelab.

---

## Project Status

**Status: Active**

Wazuh is currently monitoring both Windows and Linux endpoints in my Proxmox lab.

---

## Skills Demonstrated

- Wazuh SIEM
- Security monitoring
- Windows event analysis
- Linux monitoring
- Endpoint agents
- Authentication-event analysis
- Alert investigation
- Centralized logging
- Troubleshooting security telemetry
