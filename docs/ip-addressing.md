# IP Addressing Scheme

## Network Summary

| Network | Subnet | Role |
|---|---|---|
| Home Network | 192.168.1.0/24 | Uplink — not managed |
| Internal (transit) | 10.10.10.0/24 | FortiGate Internal2 ↔ switch link; switch's own default route points here |
| Server | 10.10.20.0/24 | Proxmox host |
| HR | 10.10.30.0/24 | Reserved, no hosts |
| IT | 10.10.40.0/24 | Reserved, no hosts |
| Business | 10.10.50.0/24 | Reserved, no hosts |
| Management | 10.10.99.0/24 | FortiGate Internal1 ↔ switch link; admin/laptop access |

---

## FortiGate 60E Interfaces

| Interface | Name | IP Address | Connects to |
|---|---|---|---|
| WAN1 | Outside Going to HRouter | 192.168.1.2/24 | Home router |
| Internal1 | Management Link | 10.10.99.2/24 | Switch G1/0/9 |
| Internal2 | Internal Link to Switch | 10.10.10.2/24 | Switch G1/0/1 |
| DMZ | dmz | 10.10.60.1/24 | Configured, unused |

---

## Catalyst 3650 SVIs

| VLAN | SVI | Role |
|---|---|---|
| 10 | 10.10.10.1 | Transit to FortiGate Internal2; switch's own next-hop for its default route |
| 20 | 10.10.20.1 | Server gateway |
| 30 | 10.10.30.1 | HR gateway (reserved) |
| 40 | 10.10.40.1 | IT gateway (reserved) |
| 50 | 10.10.50.1 | Business gateway (reserved) |
| 99 | 10.10.99.1 | Management gateway |

`ip routing` is enabled — unlike the ASA-based design, the switch now does
inter-VLAN routing directly instead of leaving it to the firewall.

---

## Routing

| Device | Route | Via |
|---|---|---|
| Switch | 0.0.0.0/0 | 10.10.10.2 (FortiGate Internal2) |
| FortiGate | 10.10.0.0/16 | 10.10.10.1 (switch VLAN 10 SVI), out Internal2 |
| FortiGate | 0.0.0.0/0 | 192.168.1.1 (home router), out WAN1 |

---

## NAT and Firewall Policy

| Policy | Source | Destination | Service | NAT |
|---|---|---|---|---|
| Inside to Outside | Internal (10.10.0.0/16) | all | PING, DNS, HTTPS | Overload, outgoing interface address |

There's no outside-to-inside policy — FortiGate's stateful session tracking
handles return traffic without one. The destination was originally scoped to
the home router's own subnet (192.168.1.0/24) rather than "all," which let
pings to on-link devices succeed while silently dropping anything actually
bound for the internet. See the [NAT troubleshooting write-up](../write-ups/outbound-nat-troubleshooting.md)
for the full diagnosis.

---

## Notes

- FortiGate's default DNS (FortiGuard, 96.45.45.45 / 96.45.46.46) was
  unreachable on this unregistered unit; DNS is set to 8.8.8.8 instead
- The ASA's old outside address (192.168.1.2) was reclaimed and reused on the
  FortiGate's WAN1 once removed from the ASA's static config
- DMZ interface is configured (10.10.60.0/24) but nothing is cabled to it yet
