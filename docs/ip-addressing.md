# IP Addressing Scheme

## Network Summary

| Network | Subnet | Role |
|---|---|---|
| Home Network | 192.168.1.0/24 | Uplink — not managed |
| Inside (Lab) | 192.168.10.0/24 | Internal lab network |
| DMZ | 192.168.20.0/24 | Demilitarized zone |

---

## ASA 5505 Interfaces

| Interface | nameif | Security Level | IP Address | Subnet Mask | Physical Port |
|---|---|---|---|---|---|
| Vlan1 | outside | 0 | 192.168.1.2 | 255.255.255.0 | Ethernet0/0 |
| Vlan10 | inside | 100 | 192.168.10.1 | 255.255.255.0 | Ethernet0/1 |
| Vlan20 | dmz | 50 | 192.168.20.1 | 255.255.255.0 | Ethernet0/2 (shut) |

On the 5505 the physical ports are switchports — addresses are assigned to the
VLAN interfaces, not to the Ethernet ports themselves. Ethernet0/2 is currently
shut down with no DMZ host attached, so Vlan20 reads down/down.

VLAN 20 carries `no forward interface Vlan1` — the Base license permits only two
fully routed interfaces, so DMZ traffic cannot go directly out and must route
through inside.

---

## Catalyst 3650

| Interface | VLAN | IP Address | Subnet Mask | Role |
|---|---|---|---|---|
| Vlan10 | Inside | 192.168.10.2 | 255.255.255.0 | Management only |
| Vlan1 | — | none | — | Shut down, unused |

`ip routing` is disabled. The SVI is a management address for SSH and syslog — it
is not a gateway and forwards no user traffic. All inter-segment routing happens
on the ASA.

| Port | VLAN | Mode | Connects to |
|---|---|---|---|
| GigabitEthernet1/0/1 | 10 | access | ASA Ethernet0/1 |
| GigabitEthernet1/0/2 | 10 | access | 891 GigabitEthernet0 |

---

## Cisco 891 Router

| Interface | IP Address | Subnet Mask | Role |
|---|---|---|---|
| GigabitEthernet0 | 192.168.10.3 | 255.255.255.0 | Uplink to SW-3650 G1/0/2 |

The 891 has no Fa0/0. Fa0–Fa7 are an integrated Layer 2 switch and cannot take an
address directly; only Fa8 and Gi0 are routed interfaces.

---

## Routing

| Protocol | Devices | Area | Notes |
|---|---|---|---|
| OSPF 1 | ASA (RID 192.168.10.1), 891 (RID 192.168.10.3) | 0 | ASA is ASBR |
| Static | ASA outside | — | 0.0.0.0/0 via 192.168.1.1 |

The ASA holds the only static default route and injects it into OSPF with
`default-information originate`. The 891 learns it as `O*E2 0.0.0.0/0` and has no
static routes of its own.

---

## Default Gateways

| Device | Default Gateway |
|---|---|
| ASA Outside | 192.168.1.1 (home router) |
| Catalyst 3650 | 192.168.10.1 (ASA inside) |
| Cisco 891 | 192.168.10.1 (ASA inside) |
| DMZ hosts | 192.168.20.1 (ASA DMZ) |

---

## NAT

| Type | Source | Translated to |
|---|---|---|
| PAT | 192.168.10.0/24 (inside) | Outside interface (192.168.1.2) |

Pre-8.3 syntax — `nat (inside) 1` paired with `global (outside) 1 interface`.
No NAT configured for the DMZ yet.

--- 

## Planned Devices (via GNS3) 

| Device | Network | IP Address | Role |
|---|---|---|---|
| DMZ Host | 192.168.20.0/24 | 192.168.20.10 | DMZ server (VM) |
| Workstation | 192.168.10.0/24 | 192.168.10.10 | Inside network client (VM) |

---

## Notes

- ASA outside interface statically assigned to avoid DHCP conflicts
- Home network 192.168.1.0/24 is not managed — used as WAN uplink only
- ASA Base license limits lab to 3 VLANs — inside, outside, DMZ
