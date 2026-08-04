# Network Topology

## Logical Diagram

![Network Topology](diagrams/Network_Topology.svg)

## Physical Connections

| From | Interface | To | Interface |
|---|---|---|---|
| Home Router | LAN | ASA 5505 | Eth0/0 |
| ASA 5505 | Eth0/1 | Catalyst 3650 | G1/0/1 |
| Catalyst 3650 | G1/0/2 | Cisco 891 | Fa0/0 |

## Management Access

All devices accessed via Keyspan USA-19HS console cable on COM3.
Cable swapped manually between devices until additional cables arrive.
