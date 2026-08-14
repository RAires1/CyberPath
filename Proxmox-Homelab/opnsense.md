# OPNsense – Firewall & Lab Networking in My Proxmox Homelab

## Overview

I run **OPNsense** as a virtual firewall/router inside my Proxmox homelab.

The goal was to give the lab its own controlled network instead of placing every test machine directly on my normal home LAN.

OPNsense now acts as the main gateway for the lab and gives me a place to practise:

- Routing
- Firewall rules
- DNS forwarding
- DHCP concepts
- Network troubleshooting
- Segmentation
- Connectivity testing

---

## Environment

| Component | Configuration |
|---|---|
| Hypervisor | Proxmox VE |
| Firewall | OPNsense |
| Deployment | Virtual Machine |
| Role | Lab gateway / firewall / router |
| Clients | Windows Server, Windows 11, Ubuntu Server |
| Status | Active |

For privacy, I do not publish the real addressing of my home network. Example addresses in this documentation are illustrative only.

---

## Why I Built This

At first, most of my lab services were connected directly to the home network.

That was fine for basic testing, but I wanted a setup that felt closer to a real IT environment, where systems sit behind a firewall and depend on routing and DNS to reach each other and the Internet.

Using OPNsense gave me a central place to control the lab network and troubleshoot connectivity problems in a more realistic way.

---

## Simplified Architecture

```text
                    Internet
                       |
                       ▼
                  Home Router
                       |
                       ▼
                  Proxmox Host
                       |
                 OPNsense VM
                       |
              +--------+--------+
              |                 |
              ▼                 ▼
          WAN side          Lab LAN
                                |
                +---------------+---------------+
                |               |               |
                ▼               ▼               ▼
           Windows Server   Windows 11      Ubuntu Server
```

The lab clients use OPNsense as their gateway.

---

## Connectivity Validation

After deployment, I tested the network step by step instead of assuming everything was working.

Typical checks included:

1. Confirm the client received the expected lab-network configuration.
2. Ping the OPNsense LAN interface.
3. Test reachability between lab systems.
4. Test Internet connectivity using an external IP.
5. Test DNS resolution separately.
6. Use `nslookup` and other tools to confirm which DNS server was answering.

One useful example was separating **basic IP connectivity** from **DNS resolution**. A client could reach the gateway successfully while name resolution still failed, which helped me troubleshoot the correct layer instead of treating every problem as the same issue.

---

## DNS Troubleshooting

I used commands such as:

```powershell
ping <gateway>
nslookup microsoft.com
Resolve-DnsName microsoft.com
```

This helped me validate:

- Gateway reachability
- DNS server selection
- External name resolution
- Domain DNS behaviour

During setup, I also worked through situations where Internet reachability and DNS did not behave the same way. That gave me practical experience isolating routing, firewall and DNS problems.

---

## Troubleshooting Approach

My normal process is:

```text
No connectivity
      |
      ▼
Check client IP configuration
      |
      ▼
Ping local gateway
      |
      +---- Fails ----> Check interface / VLAN / firewall path
      |
      ▼
Ping external IP
      |
      +---- Fails ----> Check routing / NAT / firewall
      |
      ▼
Resolve external domain
      |
      +---- Fails ----> Check DNS configuration
      |
      ▼
Connectivity confirmed
```

This approach helped me avoid changing several settings at once and made troubleshooting more repeatable.

---

## What I Learned

This project gave me more practical experience with:

- Layered network troubleshooting
- Default gateways
- Routing
- DNS
- Firewall concepts
- NAT
- Virtual networking in Proxmox
- Client/server connectivity
- Separating IP problems from DNS problems

It also became the foundation for the rest of the cybersecurity lab, because Active Directory, Wazuh and the endpoint VMs all depend on stable networking.

---

## Future Improvements

I plan to continue expanding this part of the lab with:

- Additional network segments
- VLANs
- Stronger separation between server and client systems
- More restrictive firewall policies
- Better logging
- Security-monitoring use cases

---

## Project Status

**Status: Active**

OPNsense is currently used as the routing and firewall layer for my lab environment.

---

## Skills Demonstrated

- OPNsense
- Firewalling
- Routing
- DNS troubleshooting
- NAT concepts
- Proxmox virtual networking
- Connectivity testing
- Structured troubleshooting
