# Windows 11 – Domain-Joined Workstation in My Proxmox Homelab

## Overview

I run a **Windows 11** virtual machine in Proxmox as the main client workstation for my Active Directory lab.

The machine is named:

```text
WIN11-01
```

It is joined to my lab domain:

```text
aireslab.test
```

This gives me a realistic Windows endpoint for testing domain authentication, DNS, security events and SIEM monitoring.

---

## Environment

| Component | Configuration |
|---|---|
| Hypervisor | Proxmox VE |
| Operating System | Windows 11 |
| Hostname | WIN11-01 |
| Domain | aireslab.test |
| Role | Domain workstation / test endpoint |
| Monitoring | Wazuh Agent |
| Status | Active |

---

## Why I Built This

A domain controller by itself is not enough to learn how Active Directory behaves from the client side.

I wanted a workstation where I could practise:

- Joining a Windows computer to a domain
- Logging in with domain users
- Testing authentication failures
- DNS troubleshooting
- Verifying domain-controller connectivity
- Generating endpoint events for Wazuh

---

## Domain Join

The general process I used was:

1. Configure the workstation on the lab network.
2. Verify connectivity to the domain controller.
3. Confirm that internal DNS could resolve `dc01.aireslab.test`.
4. Join the machine to the `aireslab.test` domain.
5. Reboot the workstation.
6. Log in using a domain account.
7. Confirm the computer object appeared in Active Directory.
8. Move the workstation into the intended organizational structure.

---

## DNS Validation

Before troubleshooting the domain itself, I checked name resolution from the Windows client.

Commands I used included:

```powershell
ipconfig /all
nslookup dc01.aireslab.test
nslookup microsoft.com
```

This helped me verify that the workstation could resolve both the internal domain controller and external domains.

---

## Authentication Testing

Once the domain join was working, I generated authentication events in the lab.

Examples included:

- Successful domain logons
- Failed logons
- Incorrect passwords
- Testing accounts after password resets

These tests gave me events that could be reviewed later in Wazuh.

One example I observed in Wazuh was a Windows logon failure alert related to an unknown user or incorrect password.

---

## Troubleshooting

The main lesson from setting up the workstation was that domain login problems can come from several different places.

My troubleshooting flow became:

```text
Cannot log in to domain
        |
        ▼
Check network configuration
        |
        ▼
Check gateway / OPNsense reachability
        |
        ▼
Check DC01 reachability
        |
        ▼
Check DNS resolution
        |
        ▼
Check user credentials
        |
        ▼
Check domain membership
        |
        ▼
Retest
```

This helped me separate networking, DNS and authentication issues instead of changing random settings.

---

## Wazuh Integration

I installed a Wazuh agent on the Windows 11 workstation so the endpoint can send security events to the SIEM.

This lets me use the machine for practical monitoring exercises such as:

- Failed-logon detection
- Authentication-event review
- Endpoint activity monitoring
- Comparing endpoint events with what happened on the machine

---

## What I Learned

This project gave me practical experience with:

- Windows 11 administration
- Active Directory domain joining
- Domain authentication
- DNS troubleshooting
- Windows endpoint troubleshooting
- Wazuh agents
- Windows security events
- Client/server dependencies

---

## Future Improvements

I plan to expand the workstation with:

- Sysmon
- More detailed endpoint telemetry
- Group Policy testing
- Controlled attack simulations inside the lab
- More incident-response exercises

---

## Project Status

**Status: Active**

`WIN11-01` is currently joined to `aireslab.test` and connected to my Wazuh monitoring environment.

---

## Skills Demonstrated

- Windows 11
- Domain joining
- Active Directory clients
- DNS troubleshooting
- Authentication testing
- Wazuh endpoint monitoring
- Windows event analysis
