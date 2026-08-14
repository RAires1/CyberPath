# Windows Server & Active Directory – Domain Services in My Proxmox Homelab

## Overview

I deployed a **Windows Server** virtual machine in my Proxmox homelab and promoted it to a domain controller for my lab domain:

```text
aireslab.test
```

The domain controller is named:

```text
DC01
```

This project gave me practical experience with Windows Server administration, Active Directory Domain Services, DNS, domain authentication and client joining.

---

## Environment

| Component | Configuration |
|---|---|
| Hypervisor | Proxmox VE |
| Server OS | Windows Server |
| Hostname | DC01 |
| Role | Domain Controller |
| Domain | aireslab.test |
| Main Services | AD DS, DNS |
| Status | Active |

I do not publish real production credentials or sensitive network details. Any network examples shown in this repository are for documentation only.

---

## Why I Built This

I wanted a Windows environment where I could go beyond standalone machines and practise the basics of enterprise identity and authentication.

Active Directory allowed me to work with:

- Domain users
- Domain computers
- Organizational Units
- Authentication
- Password management
- DNS dependencies
- Domain joins
- Windows event logs

It also created a realistic source of security events for my Wazuh SIEM lab.

---

## Deployment Process

My general deployment process was:

1. Create the Windows Server VM in Proxmox.
2. Configure the server hostname as `DC01`.
3. Configure networking for the lab environment.
4. Install the **Active Directory Domain Services** role.
5. Promote the server to a domain controller.
6. Create the new forest and domain `aireslab.test`.
7. Allow the server to reboot after promotion.
8. Confirm AD DS and DNS services were running.
9. Create test users and organizational structure.
10. Join a Windows 11 client to the domain.

---

## Simplified Architecture

```text
                       OPNsense
                          |
                          ▼
                     Lab Network
                          |
              +-----------+-----------+
              |                       |
              ▼                       ▼
        +-------------+         +-------------+
        | DC01        |         | WIN11-01    |
        | Windows     |         | Windows 11  |
        | Server      |         | Client      |
        +------+------+         +------+------+ 
               |                       |
               +-----------+-----------+
                           |
                    aireslab.test
```

---

## Active Directory Structure

I used Active Directory Users and Computers to create and manage lab objects.

This included working with:

- User accounts
- Computer accounts
- Organizational Units
- Password resets
- Account authentication

One practical task was moving the workstation into the appropriate organizational structure and learning how protected objects behave when deletion protection is enabled.

---

## DNS and Domain Dependency

One of the most important lessons from this project was how dependent Active Directory is on DNS.

A Windows client needs to resolve the domain controller correctly before domain authentication and domain joining will work reliably.

I used tools such as:

```powershell
nslookup dc01.aireslab.test
nslookup microsoft.com
ipconfig /all
```

This let me verify both internal domain resolution and external DNS behaviour.

---

## Authentication Testing

After joining the Windows workstation to the domain, I tested domain authentication with normal and deliberately incorrect credentials.

This produced Windows authentication events that I could later review through Wazuh.

Examples included:

- Successful domain logons
- Failed logons
- Incorrect username or password scenarios
- Password changes and resets

The failed-logon testing became particularly useful once the Windows endpoint was connected to Wazuh.

---

## Troubleshooting

Some of the issues I worked through included:

- Incorrect passwords
- Domain login problems
- DNS resolution problems
- Domain controller reachability
- Protected Active Directory objects
- Verifying that the correct account context was being used

My general troubleshooting process became:

```text
Domain login/join fails
        |
        ▼
Check client network configuration
        |
        ▼
Check DC reachability
        |
        ▼
Check DNS resolution for aireslab.test / DC01
        |
        ▼
Check user credentials
        |
        ▼
Check AD object / account state
        |
        ▼
Retest authentication
```

---

## Security Monitoring Integration

The domain controller and Windows client are part of the security-monitoring environment.

Windows authentication activity gives me useful events to investigate, especially when I deliberately generate failed logons in the controlled lab.

This links the Active Directory project directly to my Wazuh SIEM work.

---

## What I Learned

This project helped me build practical experience with:

- Windows Server administration
- Active Directory Domain Services
- Domain controllers
- DNS and AD integration
- Domain users and computers
- Organizational Units
- Domain joining
- Authentication troubleshooting
- Password management
- Windows security events

The biggest lesson was that Active Directory is not an isolated service. DNS, networking, authentication and endpoint configuration all have to work together.

---

## Future Improvements

I plan to continue improving the domain with:

- Group Policy
- More realistic user and computer structure
- Additional security policies
- Sysmon endpoint telemetry
- More authentication-monitoring exercises
- Controlled incident-response scenarios

---

## Project Status

**Status: Active**

`DC01` is currently running as the domain controller for my `aireslab.test` lab domain.

---

## Skills Demonstrated

- Windows Server
- Active Directory
- AD DS
- DNS
- Domain administration
- User and computer management
- Authentication troubleshooting
- Windows event analysis
