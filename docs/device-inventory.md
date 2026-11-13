# Device Inventory

## Fortinet FortiGate 60E
| Field | Detail |
|---|---|
| **Role** | Primary perimeter firewall — NAT, routing, firewall policy |
| **Management** | Web GUI at `https://10.10.99.2` (VLAN 99) |
| **Interfaces in use** | WAN1, Internal1, Internal2 |

---

## Cisco Catalyst 3650-24PD
| Field | Detail |
|---|---|
| **Role** | Core switch — VLAN tagging, SVIs, inter-VLAN routing |
| **OS Version** | IOS-XE 16.12.5b |
| **License** | IP Base — Smart License eval |
| **RAM** | 4GB |
| **Flash** | 1.6GB |
| **Ports** | 24x PoE+ GigabitEthernet, 2x 10G SFP+ |
| **Console** | COM5 via USB direct (mini-USB) |

---

## Cisco ASA 5505 — retired from active path
| Field | Detail |
|---|---|
| **Role** | Former perimeter firewall; kept as a baselined reference |
| **OS Version** | ASA 8.2(5)26 |
| **License** | Base — 3 VLANs, 10 inside hosts |
| **RAM** | 256MB |
| **Flash** | 128MB |
| **Note** | Outside address (192.168.1.2) reclaimed and reused on the FortiGate's WAN1 |

---

## Cisco 891-K9 — reserved for branch role
| Field | Detail |
|---|---|
| **Role** | Cabled, not yet in the routing path; planned as a branch site once site-to-site IPsec VPN is configured |
| **IOS Version** | 15.1(4)M4 |
| **Ports** | 8x FastEthernet, 1x GigabitEthernet |

---

## Dell OptiPlex 3070 (Proxmox host)
| Field | Detail |
|---|---|
| **Role** | Virtualization host — future DNS, DHCP, and test-client VMs |
| **CPU** | i5-8400, 6-core |
| **RAM** | 16GB DDR4 |
| **Network** | VLAN 20 (Server), 10.10.20.2 |

---

## Management Station
| Field | Detail |
|---|---|
| **Role** | Admin/GUI access, GNS3, documentation |
| **Network** | VLAN 99 (Management), 10.10.99.10 |
| **Console adapter** | Keyspan USA-19HS (COM3) |
