# IP Addressing Scheme

## Network Summary

| Network | Subnet | Role |
|---|---|---|
| Home Network | 192.168.1.0/24 | Uplink — not managed |
| Inside (Lab) | 192.168.10.0/24 | Internal lab network |
| DMZ | 192.168.20.0/24 | Demilitarized zone |

---

## ASA 5505 Interfaces

| Interface | VLAN | IP Address | Subnet Mask | Role |
|---|---|---|---|---|
| Ethernet0/0 | VLAN 1 (Outside) | 192.168.1.200 | 255.255.255.0 | Uplink to home router |
| Ethernet0/1 | VLAN 10 (Inside) | 192.168.10.1 | 255.255.255.0 | Lab gateway |
| Ethernet0/2 | VLAN 20 (DMZ) | 192.168.20.1 | 255.255.255.0 | DMZ gateway |

---

## Catalyst 3650 SVIs

| SVI | VLAN | IP Address | Subnet Mask | Role |
|---|---|---|---|---|
| VLAN 10 | Inside | 192.168.10.2 | 255.255.255.0 | Switch management |

---

## Cisco 891 Router

| Interface | IP Address | Subnet Mask | Role |
|---|---|---|---|
| Fa0/0 | 192.168.10.3 | 255.255.255.0 | Uplink to switch |

---

## Default Gateways

| Device | Default Gateway |
|---|---|
| ASA Outside | 192.168.1.1 (home router) |
| Catalyst 3650 | 192.168.10.1 (ASA inside) |
| Cisco 891 | 192.168.10.1 (ASA inside) |
| DMZ hosts | 192.168.20.1 (ASA DMZ) |

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
