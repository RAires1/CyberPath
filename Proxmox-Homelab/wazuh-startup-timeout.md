# Wazuh Manager Startup Timeout: A SIEM That Was Down for Nine Days

**Date:** 2026-09
**Environment:** aireslab.test (Proxmox homelab)
**Area:** SIEM availability / service troubleshooting

## Scenario

I opened the Wazuh dashboard to carry on with some detection work and got "not ready yet" instead of a login page. What I thought was a five minute fix turned into finding out that the manager had not been running since 25 August, and that nothing had told me.

## Environment

| Component | Detail |
|---|---|
| Host | wazuh01 (Ubuntu Server on Proxmox) |
| Services | wazuh-indexer, wazuh-dashboard, wazuh-manager |
| Agents | WIN11-01 (Windows 11), Ubuntu Server endpoint |
| Init system | systemd |

## Working Through It

I did not know where the problem was, so I started at the bottom of the stack and worked up.

1. **Indexer.** `systemctl status wazuh-indexer` showed it was not running. I started it, and the dashboard came back far enough to load a login page. At this point I thought I was finished.
2. **API.** After logging in, the dashboard returned error 3000 and could not reach the API. So something above the indexer was still broken.
3. **Narrowing it down.** The index patterns were green and the dashboard was clearly reading from the indexer, so both of those were working. That left the manager, which is the component that actually serves the API.
4. **Logs.** `journalctl -u wazuh-manager` answered it straight away.

```text
Aug 25 07:51:56 wazuh01 systemd[1]: Starting wazuh-manager.service - Wazuh manager...
Aug 25 07:52:41 wazuh01 systemd[1]: wazuh-manager.service: start operation timed out. Terminating.
Aug 25 07:52:41 wazuh01 systemd[1]: wazuh-manager.service: Failed with result 'timeout'.
Aug 25 07:52:41 wazuh01 systemd[1]: wazuh-manager.service: Unit process 758 (wazuh-apid) remains running after unit stopped.
Aug 25 07:52:41 wazuh01 systemd[1]: wazuh-manager.service: Unit process 766 (python3) remains running after unit stopped.
Aug 25 07:52:41 wazuh01 systemd[1]: Failed to start wazuh-manager.service - Wazuh manager.
```

Two things stood out. The date was 25 August and `uptime` on the VM was nine days, so this was not something that had just happened. And systemd had given up while parts of the service were still running, which is why the environment looked half working instead of obviously dead.

## Cause

Starting the manager by hand worked first time:

```text
/var/ossec/bin/wazuh-control start
```

It loaded 8451 rules, brought `remoted` up on 1514 and finished an SCA scan in about 11 seconds. The service itself was fine. The problem was that systemd was not waiting long enough for it.

The packaged unit file sets:

```text
TimeoutSec=45
```

Loading the rulesets takes longer than 45 seconds on this VM, so systemd killed the start every time, left the orphaned `wazuh-apid` and `python3` processes behind, and marked the unit as failed.

## Fix

A drop-in override rather than editing the packaged unit file, so a package upgrade does not undo it:

```text
sudo systemctl edit wazuh-manager
```

```text
[Service]
TimeoutStartSec=300
```

```text
sudo systemctl daemon-reload
sudo systemctl restart wazuh-manager
```

Both agents came back as active. The backlog flushed through and the dashboard showed 13,613 alerts over the next 24 hours, which is the agents catching up rather than anything actually happening.

## What I Got Wrong

I stopped too early at step 1. The indexer came up, the dashboard loaded, and I assumed it was fixed because the symptom I had noticed was gone. It was not fixed. It had moved. Getting a service to respond is not the same as getting the system to work.

I also spent longer than I should have looking at the dashboard for the cause, when the dashboard was only the thing reporting the problem. The step that actually helped was working out which component owns the API and going straight to its logs.

## Lessons Learned

- A failed start does not always mean a broken application. Here the application was fine and the timeout around it was too short.
- `TimeoutSec` sets both the start and stop timeouts. `TimeoutStartSec` is the one to raise when a service simply needs longer to come up, and a drop-in override survives package updates where an edited unit file does not.
- "Unit process X remains running after unit stopped" is worth reading rather than scrolling past. It explains why a failed service can still look partly alive from the outside.
- The real problem was not the timeout. It was that the SIEM stopped collecting on 25 August and I only found out on 3 September, by chance, because I happened to open the dashboard. Monitoring that nobody monitors is worse than no monitoring, because you think you are covered.

## Next Steps

- Add a health check on the Wazuh services so a failed manager raises something instead of failing quietly. This now comes before the rest of the September work on the roadmap.
- Check whether other lab services have the same problem, where a failure at boot leaves no visible trace until I happen to open something.
- Once Sysmon is deployed, confirm that the extra event volume does not push the manager start time back up against the new timeout.
