# Network Overview

## What is this lab?

A physical home lab originally built around a Cisco ASA 5505 firewall, a
Catalyst 3650-24PD switch, and a Cisco 891 router. The design has since moved
to a Fortinet FortiGate 60E as the primary firewall, with the Catalyst 3650
handling inter-VLAN routing directly. GNS3 is used alongside the physical gear
to simulate additional segments and virtual hosts where needed.

## Goal

I hold a CCNA and am working toward CCNP Enterprise. This lab is where I build
and validate hands-on skills that go beyond certification — designing real
network segmentation, configuring firewall policies, implementing routing
protocols, and eventually automating repetitive tasks with Ansible.

The target is a Network Engineer / Infrastructure Engineer role. This lab is
how I get there.

## Roadmap

- [x] Perimeter firewall redesign — moved from ASA zones/NAT to FortiGate
  policies and NAT overload
- [x] Network segmentation — VLANs 10/20/30/40/50/99 with inter-VLAN routing
  on the Catalyst 3650
- [ ] DHCP and DNS — likely via a Proxmox-hosted VM
- [ ] Site-to-site VPN — IPsec between the FortiGate (HQ) and the 891
  (branch), once the 891 is brought into the routing path
- [ ] Centralized logging — syslog into a free SIEM
- [ ] Network automation — Ansible playbooks once the topology settles
