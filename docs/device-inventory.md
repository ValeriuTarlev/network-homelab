# Device Inventory

## Cisco ASA 5505
| Field | Detail |
|---|---|
| **Role** | Perimeter firewall, NAT, VPN termination |
| **OS Version** | ASA 8.2(5)26 |
| **License** | Base — 3 VLANs, 10 inside hosts |
| **Serial** | JMX1346Z2Y5 |
| **RAM** | 256MB |
| **Flash** | 128MB |
| **Encryption** | 3DES/AES enabled |
| **Console** | COM3 via Keyspan USB serial adapter |
| **Upgrade path** | 8.2(5) → 9.2(4) pending image acquisition |

### Interface layout
| Interface | Role |
|---|---|
| VLAN 1 | Inside (user network) |
| VLAN 2 | Outside (simulated WAN) |
| VLAN 3 | DMZ |

---

## Cisco Catalyst 3650-24PD
| Field | Detail |
|---|---|
| **Role** | Core switch, VLANs, inter-VLAN routing |
| **OS Version** | IOS-XE 16.12.5b |
| **License** | IP Base — Smart License eval (45 days from July 28, 2026) |
| **Serial** | FDO2006E03V |
| **RAM** | 4GB |
| **Flash** | 1.6GB |
| **Ports** | 24x PoE+ GigabitEthernet, 2x 10G SFP+ |
| Console | COM5 via USB direct (mini-USB) |

---

## Cisco 891-K9 Router
| Field | Detail |
|---|---|
| **Role** | WAN simulation, dynamic routing |
| **IOS Version** | 15.1(4)M4 |
| **Serial** | FTX1630831E |
| **Ports** | 8x FastEthernet, 1x GigabitEthernet |
| **Console** | COM3 via Keyspan (shared) |

---

## Management Station
| Field | Detail |
|---|---|
| **Role** | Console access, GNS3, documentation |
| **Tools** | GNS3, PuTTY, Wireshark |
| **Console adapter** | Keyspan USA-19HS (COM3) |
