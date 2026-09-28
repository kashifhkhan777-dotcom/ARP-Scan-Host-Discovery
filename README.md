# ARP Scan Lab - Local Host Discovery

## Overview
Used arp-scan to discover live hosts on a local subnet via ARP (Address Resolution Protocol). ARP scanning is more reliable than ICMP ping sweeps on local networks because hosts cannot ignore ARP requests without losing network connectivity.

## Environment
- Scanner: Kali Linux (10.0.2.15)
- Target subnet: 10.0.2.0/24 (VirtualBox NAT network)
- Tool: arp-scan 1.10.0

## Commands Used

### Live scan
sudo arp-scan 10.0.2.0/24

### Save results to file
sudo arp-scan 10.0.2.0/24 > arp_scan.txt

## Output
Interface: eth0, type: EN10MB, MAC: 08:00:27:5a:87:bc, IPv4: 10.0.2.15
Starting arp-scan 1.10.0 with 256 hosts
10.0.2.2        52:55:0a:00:02:02       (Unknown: locally administered)
10.0.2.3        52:55:0a:00:02:03       (Unknown: locally administered)
Ending arp-scan 1.10.0: 256 hosts scanned in 1.966 seconds (130.21 hosts/sec). 2 responded

## Key Findings
Two live hosts discovered:
- 10.0.2.2   - VirtualBox NAT gateway
- 10.0.2.3   - VirtualBox DNS resolver

The scanning host itself (10.0.2.15) did NOT appear in the results, which is expected - a host never sends ARP requests for its own IP address.

## Key Takeaways
1. ARP scanning is more reliable than ICMP ping sweeps on local networks because ARP cannot be blocked.
2. ARP scanning only works on the local subnet (same broadcast domain) - it cannot cross routers.
3. The scanning host is never listed in its own ARP scan results.
4. VirtualBox NAT networks are isolated at Layer 2, which limits visible hosts.
5. On a real (bridged) LAN, the same command would reveal every device - phones, laptops, printers, IoT.
6. MAC addresses shown as "locally administered" indicate virtualised adapters.

## Comparison with Ping Sweep
| Method | Protocol | Blockable | Scope | Speed |
|---|---|---|---|---|
| Ping sweep | ICMP | Yes (firewalls) | Local + routed | Slow |
| ARP scan | ARP | No | Local subnet only | Very fast |

## Tools Used
- arp-scan 1.10.0
- Kali Linux

## Evidence
- arp_scan.txt - Raw arp-scan output
