# Ubuntu Server – Linux Endpoint in My Proxmox Homelab

## Overview

I deployed an **Ubuntu Server** virtual machine in my Proxmox homelab as the main Linux endpoint for administration and security-monitoring practice.

The system sits on the same lab network as my Windows domain environment and is monitored by Wazuh.

This gives me a Linux system that I can use for:

- Command-line administration
- Networking tests
- Service troubleshooting
- Agent deployment
- Log monitoring
- Comparing Linux and Windows endpoint behaviour

---

## Environment

| Component | Configuration |
|---|---|
| Hypervisor | Proxmox VE |
| Operating System | Ubuntu Server LTS |
| Deployment | Virtual Machine |
| Role | Linux server / monitored endpoint |
| Monitoring | Wazuh Agent |
| Guest Integration | QEMU Guest Agent |
| Status | Active |

For privacy, I do not publish the real addressing or credentials used in my environment.

---

## Why I Built This

Most business environments contain a mix of Windows and Linux systems.

I wanted my lab to reflect that rather than only using Windows machines.

Ubuntu Server gives me a simple way to practise Linux administration while also creating a second type of monitored endpoint for Wazuh.

---

## Deployment

My general deployment process was:

1. Upload the Ubuntu Server ISO to Proxmox.
2. Create the virtual machine.
3. Install Ubuntu Server.
4. Configure the lab-network connection.
5. Install and enable the QEMU Guest Agent.
6. Verify network connectivity.
7. Install the Wazuh agent.
8. Confirm the agent connected successfully to the Wazuh server.

---

## QEMU Guest Agent

I enabled the QEMU Guest Agent so Proxmox could communicate more cleanly with the guest operating system.

This gives the hypervisor better visibility into the VM and improves management tasks such as:

- Guest information
- Clean shutdown handling
- VM status visibility
- IP information in Proxmox

During setup I also had to work through an administrator-password issue, which gave me another practical troubleshooting scenario rather than a perfectly smooth installation.

---

## Networking

The Ubuntu VM uses the same controlled lab network behind OPNsense.

A simplified path is:

```text
Ubuntu Server
      |
      ▼
Proxmox virtual NIC
      |
      ▼
Lab network
      |
      ▼
OPNsense
      |
      ▼
Internet / other lab systems
```

I verified the machine could communicate with the rest of the lab before installing monitoring agents.

---

## Wazuh Integration

I installed a Wazuh agent on the Ubuntu Server and verified that it appeared as an active endpoint in the Wazuh dashboard.

This lets me collect and review Linux-side events alongside the Windows workstation.

Having both operating systems connected is useful because it gives me experience with:

- Different endpoint types
- Different logging behaviour
- Agent deployment on Linux and Windows
- Centralized monitoring

---

## Troubleshooting Approach

When the system or agent does not behave as expected, I work through the problem in layers:

```text
Service / agent problem
        |
        ▼
Check VM is running
        |
        ▼
Check network configuration
        |
        ▼
Check gateway reachability
        |
        ▼
Check service status
        |
        ▼
Check logs
        |
        ▼
Restart / correct configuration
        |
        ▼
Verify from Wazuh
```

This is the same general methodology I use with Windows systems: confirm the basics first, then move upward toward the application or monitoring layer.

---

## What I Learned

This project helped me gain practical experience with:

- Ubuntu Server installation
- Linux command-line administration
- Proxmox virtual machines
- QEMU Guest Agent
- Linux networking
- Service management
- Agent deployment
- Wazuh endpoint monitoring
- Troubleshooting Linux systems

---

## Future Improvements

I plan to use the Linux endpoint for more security exercises, including:

- SSH authentication monitoring
- File-integrity monitoring
- Service failures
- Privilege-related events
- Controlled incident-response exercises

---

## Project Status

**Status: Active**

The Ubuntu Server is currently running in Proxmox and connected to my Wazuh monitoring environment.

---

## Skills Demonstrated

- Ubuntu Server
- Linux administration
- Proxmox VE
- QEMU Guest Agent
- Linux networking
- Wazuh Agent
- Centralized monitoring
- Troubleshooting
